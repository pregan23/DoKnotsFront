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

import Client from "@/services/api"
export default {
   name:'LoginUser',
   data: ()=>({
       userNameInput: '',
       passwordInput: ''
   }),
//    mounted() {
       
//    },
   methods: {
       async loginUser() {
           const res = await Client.post(`/user/login`,{
               "userName":this.userNameInput,
               "password":this.passwordInput
           })
           console.log(res.data.token)
           localStorage.setItem("token", res.data.token)
        //    if (localStorage.token) {this.isUser=true}
        //    localStorage.setItem("userName", res.data.user.userName)
        //    localStorage.setItem("avatar", res.data.user.avatar)
           this.$emit('checkSession');
           this.userNameInput=''
           this.passwordInput=''

       } 
   },
   props: {
    checkSession: { type: Function},
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