<template>
    <v-main>
      <FrameBooth msg="Welcome to Your Vue.js App"/>
    </v-main>
</template>

<script>
// @ is an alias to /src
import FrameBooth from '@/components/FrameBooth.vue'

export default {
  name: 'HomeView',
  components: {
    FrameBooth
  },
  data: () => ({
    drawer: null,
  }),
}
</script>
