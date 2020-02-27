<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <MemeList :images=images />
  </div>
</template>

<script>

import MemeList from '@/components/MemeList.vue'

export default {
  name: 'Meme',
  components: {
    MemeList
  },
  data() {
    return {
      images: Array || Object || Function,
    }
  },
  mounted() {
    this.getImages()
  },
  methods: {
    async getImages() {
      try {
        const response = await fetch('https://api.imgflip.com/get_memes')
        const data = await response.json()
        this.images = data.data.memes
        console.log(this.images);
      } catch (error) {
        console.error(error)
      }
    },
  }
}
</script>

<style scoped>
</style>
