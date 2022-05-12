<template>
    <div>
        <div v-if="doKnotsWithStreaks" id="doknot">
            <h2>Here's what you've been working on lately</h2>
            <div :key="doKnot.id" v-for="doKnot in doKnotsWithStreaks">
                <h3>Have you been {{ doKnot.habit }}?</h3>
                <h4>{{ doKnot.alternatives }}</h4>
                <div :key="streak.id" v-for="streak in doKnot.Streaks">
                    <h3>{{ streak.howLong }}</h3>
                    <h4 v-if="streak.isActive">Ongoing</h4>
                </div>
            </div>
        </div>
        <div v-if="userEntries">
            <h2>Your Entries</h2>
            <div :key="entry.id" v-for="entry in userEntries">
                <h3>{{ entry.content }}</h3>
            </div>
        </div>
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
        } catch (error) {
    console.log('something is wrong')
  }
        },
        async getUserEntries() {
            try {
                const res = await Client.get(`/entry/${this.currentUserId}`)
                this.userEntries = res.data
            } catch (error) {
    console.log('something is wrong')
  }
        } 
    },
    mounted() {
        this.getDoKnotsWithStreaks()
        this.getUserEntries()
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