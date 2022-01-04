<template>
    <button @click="$emit('showTask')" :style="{background : showaddtask ? 'black' : 'green'}" class="btn">{{ showaddtask ? "Hide" : "Add Task"}}</button>
</template>

<script>

export default{
    name: "Button",
    props:{
        showaddtask:Boolean
    }
}
</script>

