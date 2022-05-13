<template>
    <div>
        <div v-if="doKnotsWithStreaks" id="doknot">
            <h2>Here's what you've been working on lately</h2>
            <button @click="toggleNewDoKnot">Add DoKnot</button>
            <div class="newDoKnot">
                <DoKnotForm v-if="newDoKnot" @getDoKnotsWithStreaks="getDoKnotsWithStreaks"/>
            </div>
            <div :key="doKnot.id" v-for="doKnot in doKnotsWithStreaks">
                <h3>Have you been {{ doKnot.habit }}?</h3>
                <!-- <h4>{{ doKnot.alternatives }}</h4> -->
                <div  id="streak" :key="streak.id" v-for="streak in doKnot.Streaks">
                    <h3 v-if="streak.isActive">{{ streak.howLong }} Day Streak</h3>
                    <h4 v-if="streak.isActive">Ongoing</h4>
                    <button v-if="streak.isActive" @click="updateStreak(streak.id, streak.howLong)">Staying Strong!</button>
                    <button v-if="streak.isActive" @click="endStreak(streak)">Temporary Setback</button>
                    <h4 v-if="streak.isActive">Last updated {{ streak.updatedAt }}</h4>
                </div>
            </div>
        </div>
        <div v-if="userEntries">
            <h2>Your Entries</h2>
            <div id="entry" :key="entry.id" v-for="entry in userEntries">
                <h3>{{ entry.content }}</h3>
            </div>
        </div>
    </div>
</template>

<script>
import Client from "@/services/api"
import DoKnotForm from './DoKnotForm.vue'
export default {
    name: 'UserLanding',
    components: {
        DoKnotForm
    },
    data:()=>({
        doKnotsWithStreaks:null,
        userEntries:null,
        sharedStreaks:null,
        sharedEntries:null,
        newDoKnot:false
    }),
    methods: {
        async toggleNewDoKnot() {
            if(this.newDoKnot) {
                this.newDoKnot = false
            }
            else {
                this.newDoKnot = true
            }
        },
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
        },
        
        async updateStreak(streakId, streakLength) {
            try {
                streakLength+=1
                let putBody = {
                    "howLong":`${streakLength}`
                }
                const res = await Client.put(`/streak/${streakId}`, putBody )
                this.getDoKnotsWithStreaks()
                return res.data
            } catch (error) {
    console.log('something is wrong')
  }
        },

        async endStreak(streak) {
            let doKnotId = streak.doKnotId
            try {
                let putBody = {
                    "isActive":false
                }
                const res = await Client.put(`/streak/${streak.id}`, putBody)
                console.log(res.data)
                this.newStreak(doKnotId)
                
                return res.data
            } catch (error) {
    console.log('something is wrong')
  }
        },
        async newStreak(doKnotId) {
            try {
                const res = await Client.post(`/streak/${this.currentUserId}/${doKnotId}`)
                this.getDoKnotsWithStreaks()
                return res.data
            } catch (error) {
    console.log('something is wrong')
  }
        },
        // async newDoKnot() {
        //     try {
        //         let doKnotBody =
        //         const res = await Client.post(`/doknot/${this.currentUserId}/new`, doKnotBody)
        //     }
        // }
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
    #streak {
        border-style: groove;
        border-color: rgb(165, 128, 206);
        border-radius: 25px;
        margin:10px;
        background-color: rgb(140, 201, 201);
    }
    button {
        margin:2.5px
    }
    #entry {
        border-style: groove;
        border-color: rgb(165, 128, 206);
        background-color: rgb(51, 51, 124);
        border-radius: 25px;
        margin:10px;
        color: rgb(165, 128, 206);
    }
</style>