<template>
    <v-main>
      <HelloWorld msg="Welcome to Your Vue.js App"/>
    </v-main>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  },
  data: () => ({
    drawer: null,
  }),
}
</script>
