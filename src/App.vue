<template>
  <div id="app">
    <h1>App version: v1.13.0</h1>
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <div v-if="prompt">
      <div>
        A new version is found. Refresh to load it?
      </div>
      <div
        @click="upgrade"
      >
        Refresh
      </div>
      <div
        @click="prompt = false"
      >
        Later
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  created() {
    console.log("created");
    if (this.$workbox) {
      this.$workbox.addEventListener("waiting", () => {
        console.log("here");
        this.prompt = true;
      });
    }
  },

  methods: {
    async upgrade() {
      this.prompt = false;
      await this.$workbox.messageSW({ type: "SKIP_WAITING" });
    }
  },
   data() {
    return {
      prompt: false
    };
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
