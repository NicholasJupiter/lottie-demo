<template>
  <div class="home">
    <div id="lottie"></div>
    <button @click="start">start</button>
    <button @click="pause">pause</button>
    <button @click="stop">stop</button>
    <button @click="playSegments">playSegments</button>
    <div>
      <input type="text" v-model="localHref" />
    </div>
  </div>
</template>

<script>
import lottie from 'lottie-web';

export default {
  name: 'Home',
  data() {
    return {
      animationLottie: null,
      speed: 1,
      localHref: '',
    };
  },
  async mounted() {
    const animationDom = document.querySelector('#lottie');
    const { data } = await this.$axios.get('https://uxpinchina.com/voice.json');
    console.log(data);
    this.animationLottie = lottie.loadAnimation({
      container: animationDom,
      renderer: 'svg',
      loop: false,
      autoplay: false,
      name: 'NicholasJupiter',
      animationData: data,
    });
  },
  watch: {
    localHref(val) {
      this.animationLottie.setLocationHref(location.href);
    },
  },
  methods: {
    start() {
      this.animationLottie.play();
    },
    stop() {
      this.animationLottie.stop();
    },
    pause() {
      this.animationLottie.pause();
    },
    // 跳帧
    playSegments() {
      this.animationLottie.playSegments([[15, 20]], true);
    },
  },
};
</script>
<style lang="scss" scoped>
#lottie {
  width: 100px;
  height: 100px;
  margin: 10px auto;
}
button + button {
  margin-left: 10px;
}
input {
  margin: 10px;
}
</style>
