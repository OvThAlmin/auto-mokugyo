<template>
  <section class="container">
    <div>
      <h1 class="title">auto-mokugyo</h1>
      <button class="button is-medium is-primary" @click="callSound">Start</button>
    </div>
  </section>
</template>

<script>
import clickSound from "~/assets/wood-block01.mp3";

export default {
  components: {
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
  font-size: 48px;
  color: #35495e;
  letter-spacing: 1px;
}

.links {
  padding-top: 15px;
}
</style>
