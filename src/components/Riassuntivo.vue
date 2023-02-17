<script setup>
import WelcomeItem from './WelcomeItem.vue'
import QueueBox from './QueueBox.vue'
import HistoryQueueBox from './HistoryQueueBox.vue'
</script>

<script>
  export default {
    data() {
      return {
        prefix: 'A',
        q: 1,
        history: []
      }
    },
    watch: {
      q: function (newQ, oldQ) {    
          if(this.history.length < 50) {
            this.pushToHistory(oldQ); 
            this.history = [...new Set(this.history)]
            for( let i = 0; i < this.history.length; i++ ) {
              if( this.history[i] === this.q) {
                this.history.splice(i, 1)
              }
            }
          }
      }
    },
    methods: {
      increment() {
        this.q++
      },
      decrement() {
        if(this.q >= 1) {
          this.q--
        }
      },
      pushToHistory(q) {
        this.history.unshift(q);
        console.log(this.history);
      }
    }
  }
</script>

<template>

  <QueueBox :q="q" :prefix="prefix"></QueueBox>
  <button class="btn" @click="decrement">Prev</button>
  <button class="btn" @click="increment">Next</button>

  <HistoryQueueBox v-for="i in 5" :key="Math.random()" :q="this.history[i-1]"></HistoryQueueBox>
</template>
