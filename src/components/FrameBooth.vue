<style scoped>
  body {
    display: flex;
    justify-content: center;
  }

  .web-camera-container {
    margin-top: 2rem;
    margin-bottom: 2rem;
    padding: 2rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border-radius: 4px;
    width: 500px;

  }
  .camera-button {
    margin-bottom: 2rem;
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 10px 10px 10px 10px;
  }
  .web-camera-container .camera-loading {
    overflow: hidden;
    height: 100%;
    position: absolute;
    width: 100%;
    min-height: 150px;
    margin: 3rem 0 0 -1.2rem;
  }
  .bg-frame{
    background-image: url('@/assets/img/picture_frame.png');
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    width: 72%;
  }
    div[data-app='true']{
    background-image: url('@/assets/img/bg-image.jpg')!important;
  } 
</style>
<template>
  <v-container style="background-color:white; height:100%;">
    <v-row style="justify-content: center;">
  
      <!-- <button type="button" class="button is-rounded" :class="{ 'is-primary' : !isCameraOpen, 'is-danger' : isCameraOpen}" @click="toggleCamera">
          <span v-if="!isCameraOpen">Open Camera</span>
          <span v-else>Close Camera</span>
      </button> -->
      
      <v-row
          align="center"
          justify="space-around"
          class="camera-button">
        <v-btn 
        depressed
        color="success"
        @click="takePhoto"
        >
          Accept
        </v-btn>
        <router-link to="/">
        <v-btn
          depressed
          color="error"
        >
          Decline
        </v-btn>
        </router-link>
      </v-row>

    
    <v-img
      lazy-src="require('@/assets/img/picture_frame.png')"
      max-height="480"
      max-width="366"
      :src="require('@/assets/img/picture_frame.png')"
      style="position:absolute"
    ></v-img>
    <div class="web-camera-container">
    <div v-show="isCameraOpen && isLoading" class="camera-loading">
      <ul class="loader-circle">
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </div>
    
    <div v-if="isCameraOpen" v-show="!isLoading" class="camera-box" :class="{ 'flash' : isShotPhoto }">
      
      <div class="camera-shutter" :class="{'flash' : isShotPhoto}"></div>
        
      <video v-show="!isPhotoTaken" ref="camera" :width="275" :height="337.5" autoplay style="object-fit: cover; padding-left: 30px;"></video>
      
      <canvas v-show="isPhotoTaken" id="photoTaken" ref="canvas" :width="275" :height="337.5" style="padding-left: 30px;"></canvas>
    </div>
    
    <!-- <div v-if="isCameraOpen && !isLoading" class="camera-shoot">
      <button type="button" class="button" @click="takePhoto">
        <img src="https://img.icons8.com/material-outlined/50/000000/camera--v2.png">
      </button>
    </div> -->
    
    <div v-if="isPhotoTaken && isCameraOpen" class="camera-download">
      <a id="downloadPhoto" download="my-photo.jpg" class="button" role="button" @click="downloadImage">
        Download
      </a>
    </div>
    </div>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'HelloWorld',

    data: () => ({

      isCameraOpen: false,
      isPhotoTaken: false,
      isShotPhoto: false,
      isLoading: false,
      link: '#',
      ecosystem: [
        {
          text: 'vuetify-loader',
          href: 'https://github.com/vuetifyjs/vuetify-loader',
        },
        {
          text: 'github',
          href: 'https://github.com/vuetifyjs/vuetify',
        },
        {
          text: 'awesome-vuetify',
          href: 'https://github.com/vuetifyjs/awesome-vuetify',
        },
      ],
      importantLinks: [
        {
          text: 'Documentation',
          href: 'https://vuetifyjs.com',
        },
        {
          text: 'Chat',
          href: 'https://community.vuetifyjs.com',
        },
        {
          text: 'Made with Vuetify',
          href: 'https://madewithvuejs.com/vuetify',
        },
        {
          text: 'Twitter',
          href: 'https://twitter.com/vuetifyjs',
        },
        {
          text: 'Articles',
          href: 'https://medium.com/vuetify',
        },
      ],
      whatsNext: [
        {
          text: 'Explore components',
          href: 'https://vuetifyjs.com/components/api-explorer',
        },
        {
          text: 'Select a layout',
          href: 'https://vuetifyjs.com/getting-started/pre-made-layouts',
        },
        {
          text: 'Frequently Asked Questions',
          href: 'https://vuetifyjs.com/getting-started/frequently-asked-questions',
        },
      ],
    }),
    mounted(){
        this.isCameraOpen = true;
        this.createCameraElement();
    },
      methods: {
    toggleCamera() {
      if(this.isCameraOpen) {
        this.isCameraOpen = false;
        this.isPhotoTaken = false;
        this.isShotPhoto = false;
        this.stopCameraStream();
      } else {
        this.isCameraOpen = true;
        this.createCameraElement();
      }
    },
    
    createCameraElement() {
      this.isLoading = true;
      
      const constraints = (window.constraints = {
				audio: false,
				video: true
			});


			navigator.mediaDevices
				.getUserMedia(constraints)
				.then(stream => {
          this.isLoading = false;
					this.$refs.camera.srcObject = stream;
				})
				.catch(error => {
          this.isLoading = false;
          console.log(error)
					alert("May the browser didn't support or there is some errors.");
				});
    },
    
    stopCameraStream() {
      let tracks = this.$refs.camera.srcObject.getTracks();

			tracks.forEach(track => {
				track.stop();
			});
    },
    
    takePhoto() {
      if(!this.isPhotoTaken) {
        this.isShotPhoto = true;

        const FLASH_TIMEOUT = 50;

        setTimeout(() => {
          this.isShotPhoto = false;
        }, FLASH_TIMEOUT);
      }
      
      this.isPhotoTaken = !this.isPhotoTaken;
      console.log(this.$refs)
      const context = this.$refs.canvas.getContext('2d');
      context.drawImage(this.$refs.camera, 0, 0, 450, 337.5);
    },
    
    downloadImage() {
      const download = document.getElementById("downloadPhoto");
      const canvas = document.getElementById("photoTaken").toDataURL("image/jpeg")
    .replace("image/jpeg", "image/octet-stream");
      download.setAttribute("href", canvas);
    }
  }
  }
</script>
