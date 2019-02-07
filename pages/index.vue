<template>
  <section class="container">
    <div>
      <logo/>
      <h1 class="title">auto-mokugyo</h1>
      <h2 class="subtitle">My ultimate Nuxt.js project</h2>
      <button @click="callSound">Start</button>
      <button @click="stopSound">Stop</button>
      <p>count={{count}}</p>
      <div class="links">
        <a href="https://nuxtjs.org/" target="_blank" class="button--green">Documentation</a>
        <a href="https://github.com/nuxt/nuxt.js" target="_blank" class="button--grey">GitHub</a>
      </div>
    </div>
  </section>
</template>

<script>
import Logo from "~/components/Logo.vue";
import clickSound from "~/assets/wood-block01.mp3";

export default {
  components: {
    Logo
  },
  computed: {
    count() {
      return this.$store.state.counter.count;
    }
  },
  methods: {
    callSound(e) {
      // this.$store.commit("counter/add");

      const context = new AudioContext();

      //再生するバッファを準備
      const prepareBuffer = async path => {
        //2. fetch APIで音声ファイルを取得
        const res = await fetch(path);
        //ArrayBufferを取得
        const arr = await res.arrayBuffer();
        //3. 音声ファイルをデコード
        const buf = await context.decodeAudioData(arr);

        return buf;
      };

      const play = async () => {
        const source = context.createBufferSource(); //4. Sourceノードを作成
        source.buffer = await prepareBuffer(clickSound); //5. 再生するバッファを指定
        source.connect(context.destination); // SourceノードをDestinationにつなぐ
        source.loop = true;
        source.start(0); //6. 再生開始
      };

      play();
    },
    stopSound(e) {
      context.close();
    }
  }
};
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
