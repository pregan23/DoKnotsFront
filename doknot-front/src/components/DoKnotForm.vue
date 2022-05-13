<template>
  <div>
      <form @submit.prevent="createDoKnot">
          <input placeholder="Habit to break" v-model="userHabit">
          <input placeholder="List some alternatives for yourself, separated by commas" v-model="userAlts">
          <h4>Check the box if you'd like this habit and your associated streaks to be shared with others</h4>
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
        getDoKnotswithStreaks: { type:Function }
    },
    methods: {
        async createDoKnot() {
            try {
                let doKnotBody = {
                    "habit":`${this.userHabit}`,
                    "alternatives":[this.userAlts],
                    "share":this.share
                }
                const res = await Client.post(`/doknot/${this.currentUserId}/new`, doKnotBody)
                this.$emit('getDoKnotsWithStreaks')
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