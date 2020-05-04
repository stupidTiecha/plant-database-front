<template>
  <div id="app">
    <TopBanner ref="top"></TopBanner>
    <container ref="content"></container>
    <login-frame ref="loginFrame"></login-frame>
  </div>
</template>

<script>
import TopBanner from "./components/Topbanner";
import Container from "./components/Container";
import LoginFrame from "./components/LoginFrame";

export default {
  name: 'App',
  components: {
    LoginFrame,
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
