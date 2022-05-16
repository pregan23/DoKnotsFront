<template>
    <div>
        <h2>Welcome {{ currentUserName }}</h2>
        <!-- <img src={{ currentUserAvatar }} alt="Avatar" /> -->
        <div >
            <!-- <h2>Here's what you've been working on lately</h2> -->
            
            <h2 class="sechead" @click="toggleMyDoKnots()">My DoKnots</h2>
            <div class="newDoKnot">
                <DoKnotForm v-if="newDoKnot" @newStreak="newStreak" @getDoKnotsWithStreaks="getDoKnotsWithStreaks" @getSharedDoKnotsWithStreaks="getSharedDoKnotsWithStreaks" :currentUserId="currentUserId"/>
            </div>
            <div v-if="myDoKnots">
                <button @click="toggleNewDoKnot()">DoKnot Form</button>
                    <div :key="doKnot.id" v-for="doKnot in doKnotsWithStreaks">
                        <div id="doknot">
                            <h2 id="habit">Have you been {{ doKnot.habit }}?</h2>
                        
                                        
                                    
                            <button id="delete" @click="deleteDoKnot(doKnot.id)">Delete</button>
                            <br />
                            
                            <!-- <h4>{{ doKnot.alternatives }}</h4> -->
                            <div   :key="streak.id" v-for="streak in doKnot.Streaks">
                                <div id="streak" v-if="streak.isActive">
                                    <h3 >{{ streak.howLong }} Day Streak</h3>
                                    <h4>Ongoing</h4>
                                    <button  @click="updateStreak(streak.id, streak.howLong)">Staying Strong!</button>
                                    <button  @click="endStreak(streak)">Temporary Setback</button>
                                    <h4 >Last updated {{ getDate(streak.updatedAt) }}</h4>
                                    
                                    
                                </div>
                                
                            </div>
                            <h4 id="sugg">Maybe this will help: {{ pickAlt(doKnot.alternatives) }} </h4>
                        </div>
                    </div>
                </div>
            </div>
        <div>
            <h2 class="sechead" @click="toggleMyEntries()">My Entries</h2>
            <div v-if="myEntries">
            <button @click="toggleNewEntry()">Entry Form</button>
                    <EntryForm v-if="newEntry" :currentUserId="currentUserId" @getUserEntries="getUserEntries" @getSharedEntries="getSharedEntries" />
            <div>
                <div id="entry" :key="entry.id" v-for="entry in userEntries">
                    <h3>{{ entry.content }}</h3>
                    <h4>{{ getDate(entry.updatedAt) }}</h4>
                    <button @click="deleteEntry(entry.id)">Delete</button>
                    
                </div>
            </div>
            </div>
        </div>
        <div>
            <h2 class="sechead" @click="toggleShared()">The Feed</h2>
            <div v-if="shared">
                <div :key="doKnot.id" v-for="doKnot in sharedDoKnotsWithStreaks.slice(0, 5)">
                    <div id="doknot">
                                <h3 id="habit">{{ doKnot.habit }}?</h3>
                                <!-- <h4>{{ doKnot.alternatives }}</h4> -->
                                <div   :key="streak.id" v-for="streak in doKnot.Streaks">
                                    <div id="streak" v-if="streak.isActive">
                                        <h3 >{{ streak.howLong }} Day Streak</h3>
                                        <h4>Ongoing</h4>
                                        <h4 >Last updated {{ getDate(streak.updatedAt) }}</h4>
                                    </div>
                                </div>
                            </div>
                    </div>
                <div id="entry" :key="entry.id" v-for="entry in sharedEntries.slice(0, 5)">
                    <h3>{{ entry.content }}</h3>
                    <h4>{{ getDate(entry.updatedAt) }}</h4>
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
        sharedDoKnotsWithStreaks:null,
        sharedEntries:null,
        newDoKnot:false,
        newEntry:false,
        myDoKnots:false,
        myEntries:false,
        shared:false,
        date:'',
        altSuggestion:''
    }),
    methods: {

     

        pickAlt(arr) {
            this.altSuggestion = arr[Math.floor(Math.random() * arr.length)]
            console.log(this.altSuggestion)
            return this.altSuggestion
        },

        getDate(stamp) {
            let string = stamp.toString()
            console.log(string)
            let temp = string.split("T")
            this.date =temp[0]
            console.log(this.date)
            return this.date
        },

        toggleShared() {
            if(this.shared) {
                this.shared = false
            }
            else{
                this.shared = true
            }
        },

        toggleMyEntries() {
            if(this.myEntries) {
                this.myEntries = false
                
                    this.newEntry = false
                
            }
            else {
                this.myEntries = true
            }
        },

        toggleMyDoKnots() {
            if(this.myDoKnots) {
                this.myDoKnots = false
               
                
                    this.newDoKnot = false
                
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

        toggleNewEntry() {
            if(this.newEntry) {
                this.newEntry = false
            }
            else {
                this.newEntry = true
            }
        },

        toggleNewDoKnot() {
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

        async getSharedDoKnotsWithStreaks() {
            try {
                const res = await Client.get('/doknot/feed')
                this.sharedDoKnotsWithStreaks = res.data
            } catch (error) {
    console.log('something is wrong')
  }
        },

        async getSharedEntries() {
            try {
                const res = await Client.get(`/entry/feed`)
                this.sharedEntries = res.data
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
            
                const res = await Client.post(`/streak/${this.currentUserId}/${doKnotId}`)
                this.getDoKnotsWithStreaks()
                return res.data
             
        },
        async deleteDoKnot(doKnotId) {
            const deleted = await Client.delete(`/doknot/${this.currentUserId}/delete/${doKnotId}`)
            this.getDoKnotsWithStreaks()
            return deleted
        },
        
    },
    mounted() {
        this.getDoKnotsWithStreaks()
        this.getUserEntries(),
        this.getSharedDoKnotsWithStreaks(),
        this.getSharedEntries()
    },
    
    props: {
        currentUserId: { type:Number },
        currentUserAvatar: { type:String },
        currentUserName: { type: String }
    }
}
</script>

<style>
#habit {
    border-top:solid rgb(125,195,176) 3px;
    
}

#sugg {
    border-bottom:solid 2px;
    border-color:rgb(125,195,176)
}

button {
  transition-duration: 0.4s;
  border-radius:10em;
  background-color: rgb(235,245,235);
}

button:hover {
  background-color: rgb(125,195,176); 
  
}

    #streak {
        text-align: center;
        width:300px;
        border-left: 5px;
        border-right: 5px;
        border-style: groove;
        border-color: rgb(170,240,209);
        border-radius: 25px;
        /* color:rgb(51, 51, 124); */
        font-weight:900;
        
        margin:10px;
        background-color: rgb(210,205,228);
    }
    button {
        margin:2.5px
    }
    #entry {
        border-style:groove;
        border-color: rgb(170,240,209);
        background-color: rgb(235,245,235);
        border-radius: 25px;
        margin:10px;
        /* color: rgb(165, 128, 206); */
        
    }
    /* #delete {
        align-self: ;
    } */
    #doknot {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        
        
    }
    .sechead {
        border-style: double;
        
        border-color: cadetblue;
    }

    #delete {
        align-content: center;
        justify-self: center;
        height:25px
    }
    /* #streak {
        float: right;
    } */
</style>