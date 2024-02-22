<template>
    <div class = "todo-item">
        <input type="checkbox" v-model="done">
        <p>{{ content }}</p>
    </div>

</template>

<script setup>
    const props = defineProps({
        content:String,
        done:Boolean
    })
</script>

<style scoped>
    .todo-item{
        border:0.1px white;
        display: flex;
    }

    .todo-item:hover {
        box-shadow: 2px 2px #1f6089;
        cursor: pointer;
    }
</style>