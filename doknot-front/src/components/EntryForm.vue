<template>
    <div>
        <form @submit.prevent="newEntry()">
            <h5>Journaling can help us to be more deliberate and mindful of our actions.  This is a space to chronicle your journey and share it with others if you so choose.</h5>
            <input id="userEntryBox" placeholder="Your Entry Here" v-model="userContent"/>
            <h6>Check box to share with others</h6>
            <input type="checkbox" v-model="share" />
            <button type="submit">Submit</button>
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
                if(this.share) {
                    this.$emit('getSharedEntries')
                }
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
#userEntryBox {
    width:300px;
    height:200px;
    text-align: center;
}

</style>