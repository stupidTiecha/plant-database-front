<template>
  <div id="app">
    <TopBanner ref="top"></TopBanner>
    <container ref="content"></container>
  </div>
</template>

<script>
import TopBanner from "./components/Topbanner";
import Container from "./components/Container";

export default {
  name: 'App',
  components: {
    Container: Container,
    TopBanner: TopBanner,
  }
}
</script>

<style>
#app {
  margin: 0;
  overflow: hidden;
}
</style>
