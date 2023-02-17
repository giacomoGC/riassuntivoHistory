<script>
    export default {
        props: {
            q: Number
        }
    }
</script>

<template>
    <div class="historyQueueBox">
        <div class="queue historyQueue">
            <h1 v-if="q">{{ (q).toString().padStart(2,0) }}</h1>
        </div>
    </div>
</template>