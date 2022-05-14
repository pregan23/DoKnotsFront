<template>
    <div>
        <div v-if="doKnotsWithStreaks" id="doknot">
            <!-- <h2>Here's what you've been working on lately</h2> -->
            <div class="newDoKnot">
                <DoKnotForm v-if="newDoKnot" @newStreak="newStreak" @getDoKnotsWithStreaks="getDoKnotsWithStreaks" :currentUserId="currentUserId"/>
            </div>
            <h2 @click="toggleMyDoKnots">My DoKnots +</h2>
            <div v-if="myDoKnots">
                <button @click="toggleNewDoKnot">Add DoKnot</button>
                    <div :key="doKnot.id" v-for="doKnot in doKnotsWithStreaks">
                        <div id="doknot">
                            <h3>Have you been {{ doKnot.habit }}?</h3>
                            <button id="delete" @click="deleteDoKnot(doKnot.id)">Delete</button>
                            <!-- <h4>{{ doKnot.alternatives }}</h4> -->
                            <div   :key="streak.id" v-for="streak in doKnot.Streaks">
                                <div id="streak" v-if="streak.isActive">
                                    <h3 >{{ streak.howLong }} Day Streak</h3>
                                    <h4>Ongoing</h4>
                                    <button  @click="updateStreak(streak.id, streak.howLong)">Staying Strong!</button>
                                    <button  @click="endStreak(streak)">Temporary Setback</button>
                                    <h4 >Last updated {{ streak.updatedAt }}</h4>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        <div v-if="userEntries">
            <h2 @click="toggleMyEntries()">My Entries</h2>
            <div v-if="myEntries">
            <button @click="toggleNewEntry()">New Entry</button>
                    <EntryForm v-if="newEntry" :currentUserId="currentUserId" @getUserEntries="getUserEntries" />
            <div>
                <div id="entry" :key="entry.id" v-for="entry in userEntries">
                    <h3>{{ entry.content }}</h3>
                    <button @click="deleteEntry(entry.id)">Delete</button>
                    
                </div>
            </div>
            </div>
        </div>
    </div>
</template>

<script>
import Client from "@/services/api"
import DoKnotForm from './DoKnotForm.vue'
import EntryForm from "./EntryForm.vue"

export default {
    name: 'UserLanding',
    components: {
    DoKnotForm,
    
    EntryForm
},
    data:()=>({
        doKnotsWithStreaks:null,
        userEntries:null,
        sharedStreaks:null,
        sharedEntries:null,
        newDoKnot:false,
        newEntry:false,
        myDoKnots:false,
        myEntries:false
    }),
    methods: {

        async toggleMyEntries() {
            if(this.myEntries) {
                this.myEntries = false
            }
            else {
                this.myEntries = true
            }
        },

        async toggleMyDoKnots() {
            if(this.myDoKnots) {
                this.myDoKnots = false
            }
            else {
                this.myDoKnots = true
            }
        },
        
        async deleteEntry(id) {
            try {
                const deleted = await Client.delete(`/entry/${this.currentUserId}/delete/${id}`)
                this.getUserEntries()
                return deleted.data
            } catch (error) {
    console.log('something is wrong')
  }
        },

        async toggleNewEntry() {
            if(this.newEntry) {
                this.newEntry = false
            }
            else {
                this.newEntry = true
            }
        },

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
        async deleteDoKnot(doKnotId) {
            const deleted = await Client.delete(`/doknot/${this.currentUserId}/delete/${doKnotId}`)
            this.getDoKnotsWithStreaks()
            return deleted
        },
        
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
        text-align: center;
        width:50%;
        border-left: 5px;
        border-right: 5px;
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
        border-style:groove;
        border-color: rgb(165, 128, 206);
        background-color: rgb(51, 51, 124);
        border-radius: 25px;
        margin:10px;
        color: rgb(165, 128, 206);
    }
    /* #delete {
        align-self: ;
    } */
    #doknot {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        
        
    }
    /* #streak {
        float: right;
    } */
</style>