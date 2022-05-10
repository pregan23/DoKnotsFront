<template>
<div>
    <form class="regform">
        <input placeholder='Username'/>
        <input placeholder="Password" />
    </form>
</div>
</template>

<script>
export default {
   name:'RegisterUser',
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