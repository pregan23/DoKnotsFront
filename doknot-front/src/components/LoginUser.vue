<template>
<div>
    <h3>Login Here</h3>
    <form class="logform" @submit.prevent="loginUser()">
        <input placeholder='Username' v-model="userNameInput"/>
        <input placeholder="Password" v-model="passwordInput"/>
        <button type="submit">Login</button>
    </form>
</div>
</template>

<script>
import axios from 'axios'
export default {
   name:'LoginUser',
   data: ()=>({
       userNameInput: '',
       passwordInput: ''
   }),
   methods: {
       async loginUser() {
           const res = await axios.post('http://localhost:3001/user/login',{
               "userName":this.userNameInput,
               "password":this.passwordInput
           })
           console.log(res.data.user)
           localStorage.setItem("token", res.data.token)
        //    if (localStorage.token) {this.isUser=true}
        //    localStorage.setItem("userName", res.data.user.userName)
        //    localStorage.setItem("avatar", res.data.user.avatar)
           this.userNameInput=''
           this.passwordInput=''
       } 
   },
   props: {
    //    ['isUser']
   } 
}
</script>

<style scoped>
.logform {
    display: flex;
    flex-direction: column;
    align-items: center;
    }
input {
    width: 30%;
    text-align: center;
}
</style>