<script>
    export default {
        props: {
            q: Array
        }
    }
</script>

<template>
    <div class="historyQueueBox">
        <div class="queue historyQueue">
            <h2 v-if="q">{{ `${(q[0])} ${(q[1]).toString().padStart(2,0)}` }}</h2>
        </div>
    </div>
</template>