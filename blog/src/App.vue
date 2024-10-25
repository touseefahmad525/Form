<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <!-- <SubmitForm/> -->
    <!-- <TEST/> -->
     <YouTube/>
</template>

<script>
// import SubmitForm from './components/SubmitForm.vue';
import YouTube from './components/YouTube.vue';
// import TEST from './components/TEST.vue';


// import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'App',
  components: {
    // HelloWorld,
    // SubmitForm,
    // TEST,
    YouTube
  },
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
