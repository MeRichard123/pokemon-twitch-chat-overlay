<template>
    <div :class="[isSub ? 'sub' : '','card']">
        <span><span class="strong">{{username}}</span>: {{message}}</span>
    </div>
</template>

<script>
export default {
  name: 'Chat-Card',
  props: {
    username: String,
    message: String,
    isSub: Boolean,
  },
};
</script>

<style scoped>
.card{
    border-bottom: 2px solid rgb(83, 79, 79);
    padding: 1rem 2rem;
    margin: 0 10px;
    text-align: center;
    background: black;
    color: white;
}
.sub{
    background: rgb(87, 87, 87);
}
.strong{
    font-weight: 900;
}
</style>
