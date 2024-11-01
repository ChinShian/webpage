<template>
  <img alt="Vue logo" src="@/assets/images/logo.png" />
  <HelloWorld msg="Welcome to Your Vue.js App" />
  <BoxWindow />
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import BoxWindow from "./components/BoxWindow.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    BoxWindow,
  },
};
</script>

<style lang="scss">
@import "assets/scss/reset.scss";
html {
  font-size: 10px;
}
body {
  font-size: 1.6rem;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: relative;
  padding: 60px 0 0;
}
</style>
