<template>
<div>
    <form class="regform">
        <input placeholder='Username' v-model="userNameInput"/>
        <input placeholder="Password" v-model="passwordInput"/>
    </form>
</div>
</template>

<script>
export default {
   name:'RegisterUser',
   data: ()=>({
       userNameInput: '',
       passwordInput: ''
   }),
   methods: {

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