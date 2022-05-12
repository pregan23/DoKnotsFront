<template>
    <div>
        <h1>Placeholder</h1>
    </div>
</template>

<script>
import Client from "@/services/api"
export default {
    name: 'UserLanding',
    data:()=>({
        doKnotsWithStreaks:null,
        userEntries:null,
        sharedStreaks:null,
        sharedEntries:null
    }),
    methods: {
        async getDoKnotsWithStreaks() {
            try {
            
            const res = await Client.get(`/doknot/${this.currentUserId}`)
            console.log(res.data)
            this.doKnotsWithStreaks = res.data
            return this.doKnotsWithStreaks
        } catch (error) {
    console.log('no token')
  }
        } 
    },
    mounted() {
        this.getDoKnotsWithStreaks()
        // getUserEntries(),
        // getSharedStreaks(),
        // getSharedEntries()
    },
    
    props: {
        currentUserId: { type:Number }
    }
}
</script>

<style>

</style>