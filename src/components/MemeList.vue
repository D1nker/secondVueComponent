<template>
<div class="memebox">
  <div
    v-for="image in images"
    :key="image.id"
    :style="{ backgroundImage: 'url(' + image.url + ')' }"
    class="meme"
    @click="copy(image)"
    @mouseleave="hover"
  >
    <img :src="image.url" />
  </div>
</div>
</template>

<script>
export default {
  name: 'MemeList',
  props: {
    images: Array || Function || Object,
  },
  data() {
    return {
      clicked: false,
    }
  },
  methods: {
    copyToClipboard(text) {
      console.log('allo');
      const textArea = document.createElement('textarea');
      textArea.value = text;
      document.body.appendChild(textArea);
      textArea.select();
      document.execCommand('copy');
      document.body.removeChild(textArea);
    },
    copy(image) {
      console.log(image);
      this.copyToClipboard(image.url)
      this.clicked = true;
    },
    hover() {
      this.clicked = false;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

* {
  box-sizing: border-box;
}

html,
body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

body {
  background-color: #333333;
  font-family: sans-serif;
}
.memebox {
  display: flex;
  flex-flow: row wrap;
}

.meme img {
  max-width: 100%;
}

.meme {
  flex: 1 0 10%;
  position: relative;
  min-width: 175px;
  min-height: 175px;
  background-position: center;
  background-size: cover;
}

.meme:before {
  content: "Copy URL";
  display: none;
  position: absolute;
  z-index: 1;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.75);
  line-height: 175px;
  text-align: center;
  color: white;
  cursor: pointer;
}

.meme:hover:before {
  display: block;
}

.meme-clicked:before {
  content: "Copied!";
}
</style>
