<template>
  <div id="app">
    <TopBanner ref="top"></TopBanner>
    <container ref="content"></container>
    <login-frame ref="loginFrame"></login-frame>
    <div id="contactUs">
      <p style="width: inherit"><span style="font-weight: bold">Phone :</span> +27 21 461 9265
        <span style="font-weight: bold">Email : </span><span style="color: darkolivegreen">info@greenpop.org </span>
        <span style="font-weight: bold">Business Hours: </span> 8:00am - 5:00pm, Monday to Friday
        <span style=" font-weight: bold">   Copyright@2020</span>
      </p>
    </div>
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
#contactUs {
  width: inherit;
  font-family: "Times New Roman",serif;
  height: 30px;
  line-height: 30px;
  font-size: 22px;
  position: fixed;
  bottom: 0;
  right: 0;
  left: 0;
  text-align: justify;
  border-color: black;
  border-top: 1px ;
  border-bottom: 1px ;
  border-left: 0 ;
  border-right: 0 ;
  border-style: solid;
  background-color: rgba(210,213,221,1);
}
/*#contactUs >>> span {*/
/*  margin: 0 10px 0 10px;*/
/*}*/
</style>
