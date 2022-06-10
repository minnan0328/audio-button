<template>
  <div id="app">
    <audioButton :source="musicSource_4m" :disabled="true" />
    <audioButton :source="musicSource_30s" />
    <audio :src="musicSource_30s" controls></audio>
  </div>
</template>

<script>
import audioButton from "./components/audioButton/audioButton";

let musicSource_4m = require("./assets/music/AsYouFadeAway-NEFFEX.mp3");
let musicSource_30s = require("./assets/music/EasyLemon30Second-KevinMacLeod.mp3");

export default {
  name: "App",
  components: {
    audioButton,
  },
  data() {
    return {
      musicSource_4m: musicSource_4m,
      musicSource_30s: musicSource_30s,
    };
  },
};
</script>

<style lang="scss" >
</style>
