<script setup>
import WelcomeItem from './WelcomeItem.vue'
import QueueBox from './QueueBox.vue'
import HistoryQueueBox from './HistoryQueueBox.vue'
</script>

<script>
  export default {
    data() {
      return {
        prefixes: ['A', 'B'],
        q1: 1,
        q2: 1,
        history: []
      }
    },
    methods: {
      // We need to set one watcher for each queue, with same callback
      // For each queue of the queues array (passed as parameter) we store prefix, previous call and current call,
      // then we compare each history element with the newly called queue, then duplicates are removed from history
      setupWatchers(queues) {
        console.log('watchers setup')
        for (let i in queues) {
          let key = queues[i];

          this.$watch(key, function (newQ, oldQ) {  
              this.pushToHistory([this.prefixes[i], oldQ]);
              
              // Check for duplicate objects inside history array
              // We convert the array of arrays into an array of strings and use Filter methods on it
              this.history = Array.from(new Set(this.history.map(JSON.stringify)), JSON.parse)

              // Check for duplicates between main queues and history ones
              // Each element of the history (history[i]) is compared to the newly called queue
              // and duplicates are removed from history
              for( let subIndex = 0; subIndex < this.history.length; subIndex++ ) {
                if( JSON.stringify(this.history[subIndex]) === JSON.stringify([this.prefixes[i], newQ]) ) {
                  this.history.splice(subIndex, 1)
                }
              }

            // TODO: different action if history length is getting too long..  
            if(this.history.length > 50) {
              let elementsToDelete = 25;
              while (elementsToDelete--)
              // Pop the last element from the
              // end of the array
              this.history.pop();
            
              console.log("Modified Array:", this.history.length);
            }
          })
        }
      },
      increment(queue) {
        this[queue]++
      },
      decrement(queue) {
        if(this[queue] >= 1) {
          this[queue]--
        }
      },
      pushToHistory(q) {
        this.history.unshift(q);
      }
    },
    created() {
      this.setupWatchers(['q1', 'q2']);
    }
  }
</script>

<template>
  <div style="display: flex">
    <div>
      <QueueBox :q="q1" :prefix="this.prefixes[0]" :key="1"></QueueBox>
      <button class="btn" @click="decrement('q1')">Prev</button>
      <button class="btn" @click="increment('q1')">Next</button>
    </div>
    <div>
      <QueueBox :q="q2" :prefix="this.prefixes[1]" :key="2"></QueueBox>
      <button class="btn" @click="decrement('q2')">Prev</button>
      <button class="btn" @click="increment('q2')">Next</button>
    </div>
  </div>

  <HistoryQueueBox v-for="i in 5" :key="Math.random()" :q="this.history[i-1]"></HistoryQueueBox>
</template>
