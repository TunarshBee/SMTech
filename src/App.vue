<template>
  <img alt="Vue logo" class='logo' src="./assets/star_wars_logo.png">
  <LandingPage/>
</template>

<script>
import LandingPage from './components/LandingPage.vue';



export default {
  name: 'App',
  components: {
    LandingPage
}
}
</script>

<style>
body{

  background: #000000;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ff0;
  margin-top: 60px;
  padding: 2%;
}
.logo{
  width: 100%;
}
</style>
