<template>
  <div>
      <form @submit.prevent="createDoKnot">
          <input placeholder="Habit to break" v-model="userHabit">
          <input placeholder="Alternatives/Strategies" v-model="userAlts">
          <h4>Check the box if you'd like this habit and your associated streaks to be shared with others.  Alternatives/Strategies are always private</h4>
          <input type="checkbox" v-model="share">
          <button type="submit">Add</button>
      </form>
  </div>
</template>

<script>
import Client from '../services/api'
export default {
    name:'DoKnotForm',
    data:()=>({
        userHabit: '',
        userAlts: [],
        share: false
    }),
    props: {
        currentUserId: { type:Number },
        getDoKnotswithStreaks: { type:Function },
        newStreak: { type:Function }
    },
    methods: {
        async createDoKnot() {
            try {
                let id = parseInt(this.currentUserId)
                let altArray = this.userAlts.split(', ')
                let doKnotBody = {
                    "habit":`${this.userHabit}`,
                    "alternatives":altArray,
                    "share":this.share
                }
                console.log(id)
                const res = await Client.post(`/doknot/${id}/new`, doKnotBody)
                this.$emit(`newStreak`, res.data.id)                
                this.userHabit='',
                this.userAlts=[],
                this.share=false
                return res.data
            } catch (error) {
    console.log('something is wrong')
  }
        }
    }
}
</script>

<style>

</style>