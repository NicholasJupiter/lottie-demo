<template>
  <div class="home">
    <div id="lottie"></div>
    <button @click="start">start</button>
    <button @click="pause">pause</button>
    <button @click="stop">stop</button>
    <button @click="playSegments">playSegments</button>
    <button @click="goToAndStop">goToAndStop</button>
    <button @click="goToAndPlay">goToAndPlay</button>
    <button @click="getDuration">getDuration</button>
    <button @click="print">print</button>
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
      direction: 1,
    };
  },
  async mounted() {
    const animationDom = document.querySelector('#lottie');
    const { data } = await this.$axios.get('https://uxpinchina.com/demo.json');
    this.animationLottie = lottie.loadAnimation({
      container: animationDom,
      renderer: 'svg',
      loop: false,
      autoplay: false,
      name: 'voice',
      animationData: data,
    });
    window.ani = this.animationLottie;
    this.animationLottie.setSubframe(false);
    console.log(this.animationLottie);
    // ['enterFrame', 'segmentStart', 'config_ready', 'data_ready', 'DOMLoaded', 'destroy'].forEach(
    //   (k) => {
    //     this.animationLottie.addEventListener(k, (res) => {
    //       // console.log(k, res);
    //     });
    //   },
    // );
    this.animationLottie.addEventListener('segmentStart', (res) => {
      // console.log('segmentStart', res);
    });
    this.animationLottie.addEventListener('data_ready', (res) => {
      // console.log(res);
    });
    this.animationLottie.addEventListener('enterFrame', (res) => {
      // console.log(res);
    });
    this.animationLottie.addEventListener('complete', (res) => {
      // 设置播放顺序
      const dir = this.animationLottie.playDirection;
      this.animationLottie.setDirection(dir === 1 ? -1 : 1);
      this.start();
      // console.log(this.animationLottie);
      // this.getDuration() === this.animationLottie;
      // console.log('complete', res);
      // 1. 播放指定帧,顺序倒过来
      // this.playSegments(this.direction === 1 ? [this.getDuration(), 0] : [0, this.getDuration()]);
      // this.direction = this.direction === 1 ? -1 : 1;
    });
  },
  methods: {
    print() {
      console.log(this.animationLottie);
    },
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
    playSegments(segments) {
      this.animationLottie.playSegments(segments, true);
    },
    goToAndStop() {
      this.animationLottie.goToAndStop(3000, false); // true = time  false = frame
    },
    goToAndPlay() {
      this.animationLottie.goToAndPlay(178, false); // true = time  false = frame
    },
    getDuration() {
      const duration = this.animationLottie.getDuration(true);
      return duration;
    },
  },
};
</script>
<style lang="scss" scoped>
#lottie {
  width: 400px;
  height: 400px;
  margin: 10px auto;
}
button + button {
  margin-left: 10px;
}
input {
  margin: 10px;
}
</style>
