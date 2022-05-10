<template>
  <div id="app">
    <h1>Welcome. Log in or Register Below</h1>
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <RegisterUser />
  </div>
</template>


<script>
// import HelloWorld from './components/HelloWorld.vue'
import RegisterUser from './components/RegisterUser.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld,  
    RegisterUser
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
