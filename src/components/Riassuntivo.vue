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
        prefix2: 'B',
        q1: 1,
        q2: 1,
        history: []
      }
    },
    watch: {
      q1: function (newQ, oldQ) {    
          if(this.history.length < 50) {
            this.pushToHistory(oldQ); 
            this.history = [...new Set(this.history)]
            for( let i = 0; i < this.history.length; i++ ) {
              if( this.history[i] === this.q1) {
                this.history.splice(i, 1)
              }
            }
          }
      }
    },
    methods: {
      increment(queue) {
        this.queue++
      },
      decrement(queue) {
        if(this.queue >= 1) {
          this.queue--
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

  <div style="display: flex">
    <div>
      <QueueBox :q="q1" :prefix="prefix" :key="1"></QueueBox>
      <button class="btn" @click="decrement(q1)">Prev</button>
      <button class="btn" @click="increment(q1)">Next</button>
    </div>
    <div>
      <QueueBox :q="q2" :prefix="prefix2" :key="2"></QueueBox>
      <button class="btn" @click="decrement(q2)">Prev</button>
      <button class="btn" @click="increment(q2)">Next</button>
    </div>
  </div>


  <HistoryQueueBox v-for="i in 5" :key="Math.random()" :q="this.history[i-1]"></HistoryQueueBox>
</template>
