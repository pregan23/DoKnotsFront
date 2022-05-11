<template>
<div>
    <h3>Register Here</h3>
    <form class="regform" @submit.prevent="registerUser">
        <input placeholder='Username' v-model="userNameInput"/>
        <input placeholder="Password" v-model="passwordInput"/>
        <button type="submit">Register</button>
    </form>
</div>
</template>

<script>
import axios from 'axios'
export default {
   name:'RegisterUser',
   data: ()=>({
       userNameInput: '',
       passwordInput: ''
   }),
   methods: {
       async registerUser() {
           const res = await axios.post('http://localhost:3001/user/new', {
               "userName":this.userNameInput,
               "password":this.passwordInput
           })
           console.log(res.data.user)
           this.userNameInput=''
           this.passwordInput=''
       }
   },
   props: {} 
}
</script>

<style scoped>
.regform {
    display: flex;
    flex-direction: column;
    align-items: center;
    }
input {
    width: 30%;
    text-align: center;
}
</style>