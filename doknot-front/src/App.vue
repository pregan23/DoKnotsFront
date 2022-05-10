<template>
  <div id="app">
    <h1>Welcome. Log in or Register Below</h1>
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <RegisterUser />
    <LoginUser />
   
  </div>
</template>


<script>
// import HelloWorld from './components/HelloWorld.vue'
import RegisterUser from './components/RegisterUser.vue'
import LoginUser from './components/LoginUser.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld,  
    RegisterUser,
    LoginUser
},
  data:()=>({
    user: {}
  })
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
