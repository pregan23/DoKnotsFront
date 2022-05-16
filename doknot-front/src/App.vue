<template>
  <div id="app">
    <div class="hastoken" v-if="authenticated">
      <button @click="logoutUser()">Log Out</button>
      <h1>What you Do is just as important as what you DoKnot</h1>
        
      <UserLanding :currentUserId="currentUserId"  :currentUserName="currentUserName" />
    </div>

    <div class="notoken" v-else>
    <h1>Welcome. Log in or Register Below</h1>
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <RegisterUser />
    <LoginUser @checkSession="checkSession"/>
    </div>
  </div>
</template>


<script>
// import HelloWorld from './components/HelloWorld.vue'
import RegisterUser from './components/RegisterUser.vue'
import LoginUser from './components/LoginUser.vue'

// import axios from 'axios'
import Client from './services/api'
import UserLanding from './components/UserLanding.vue'



export default {
  name: 'App',
  components: {
    // HelloWorld,  
    RegisterUser,
    LoginUser,
    UserLanding
},
  data:()=>({
    authenticated:false,
    currentUserName:'',
    currentUserId:null,
    currentUserAvatar: ''
  }),
  mounted() {
    const token = localStorage.getItem('token')
    if(token) {
      this.checkSession()
    }
    // this.checkSession() 
  },
  methods: {
    logoutUser() {
      this.currentUserName = ''
      this.currentUserId = null
      this.currentUserAvatar = ''
      this.authenticated = false
      localStorage.clear()
      this.checkSession()
    },


    async checkSession() {
       try {
    
    const res = await Client.get(`/user/session`)
    console.log('token checked successfully')
    console.log(res.data)
    this.authenticated = true
    this.currentUserName = res.data.userName
    this.currentUserId = res.data.id
    // this.currentUserAvatar = toString(res.data.avatar)
    return res.data
      
  } catch (error) {
    console.log('no token')
  }
    }
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100&display=swap');


#app {

  display: flex;
  align-content: center;
  position: relative;
  
  font-family: 'Montserrat', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #ed8728; */
  margin: 1px;
  background-color: rgb(204,179,219);
  border: solid 3px rgb(125,195,176);
  height:100%;
  min-height: 883px;
  
  
  

}

</style>
