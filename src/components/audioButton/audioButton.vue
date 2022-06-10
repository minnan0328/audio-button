<template src="./audioButton.html"></template>
<style lang="scss" scoped src="./audioButton.scss"></style>

<script>
const audioState = {
  play: "play",
  pause: "pause",
  stop: "stop",
};

const stateText = {
  play: "試聽",
  pause: "暫停",
  stop: "停止",
};

export default {
  name: "audioButton",
  props: {
    source: {
      type: String,
      default: null,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      currentAudioState: null,
      audio: null,
      audioInfo: {
        duration: 0,
        currentTime: 0,
        volume: 0,
        paused: Boolean,
        ended: Boolean,
        readyState: null,
        percent: 0,
      },
    };
  },
  computed: {
    btnState() {
      return {
        play: {
          text: stateText.play,
          event: () => this.playAudio(),
        },
        pause: {
          text: stateText.pause,
          event: () => this.pauseAudio(),
        },
      };
    },
    isPlaying() {
      return (
        this.currentAudioState === audioState.pause &&
        this.audioInfo.paused === false &&
        this.audioInfo.ended === false &&
        this.audioInfo.currentTime !== 0 &&
        this.audioInfo.currentTime !== this.audioInfo.duration
      );
    },
    isPaused() {
      return (
        this.currentAudioState === audioState.play &&
        this.isPlaying === false &&
        this.audioInfo.paused &&
        this.audioInfo.currentTime > 0
      );
    },
  },

  methods: {
    init() {
      this.audio = new Audio();
      this.audio.load();

      this.audio.src = this.source;

      this.audio.addEventListener("timeupdate", this.getCurrentTime);
    },
    playAudio() {
      this.audio.play();
      this.currentAudioState = audioState.pause;
    },
    pauseAudio() {
      this.audio.pause();
      this.currentAudioState = audioState.play;
    },
    stopAudio() {
      this.pauseAudio();
      this.audio.currentTime = 0;
    },
    getAudioInfo() {
      this.audioInfo.duration = this.audio.duration;
      this.audioInfo.currentTime = this.audio.currentTime;
      this.audioInfo.volume = this.audio.volume;
      this.audioInfo.paused = this.audio.paused;
      this.audioInfo.ended = this.audio.ended;
      this.audioInfo.readyState = this.audio.readyState;

      if (this.audio.currentTime === this.audio.duration) {
        this.stopAudio();
      }
    },
    getCurrentTime() {
      this.getAudioInfo();
      this.audioInfo.percent =
        this.audio.currentTime > 0 && this.audio.duration > 0
          ? `${(this.audio.currentTime / this.audio.duration) * 100}%`
          : "0%";
    },
  },
  created() {
    this.currentAudioState = audioState.play;
    this.init();
  },
};
</script>

