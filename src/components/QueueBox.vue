<script>
    export default {
        props: {
            prefix: String,
            q: Number
        }
    }
</script>

<template>
    <div class="queueBox">
        <div class="queue">
            <h1>{{ prefix }} {{ (q).toString().padStart(2,0) }}</h1>
        </div>
    </div>
</template>