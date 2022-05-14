<template>
    <div>
        <form @submit.prevent="newEntry()">
            <h1>Hello!?</h1>
            <input placeholder="Your Entry Here" v-model="userContent"/>
            <input type="checkbox" v-model="share" />
            <button type="submit"></button>
        </form>
    </div>
  
</template>

<script>
import Client from "@/services/api"

export default {
    name: 'EntryForm',
    data:()=>({
        userContent: '',
        share:false
    }),
    methods: {
        async newEntry() {
            try {
                let entryBody = {
                    "content":this.userContent,
                    "share":this.share
                }
                const entry = await Client.post(`/entry/${this.currentUserId}/new`, entryBody)
                this.$emit('getUserEntries')
                this.userContent = ''
                return entry.data
            }  catch (error) {
    console.log('something is wrong')
  }
            
        }
    },
    
    props:{
        currentUserId: { type:Number },
        getDoKnotswithStreaks: { type:Function }
    }
}
</script>

<style>

</style>