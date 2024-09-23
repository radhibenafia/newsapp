<template>
    <div>
        <h2>Component Two</h2>
        <input type="text" v-model="msg" placeholder="Write something...">
        <p>Your message:</p>
        <p><strong>{{ msg }}</strong></p>
    </div>
</template>

<script>
export default {
    name: 'CompTwo',
    data() {
        return {
            msg: ''
        }
    }
}
</script>

<style scoped>
div {
    background-color: lightpink;
}

strong {
    background-color: yellow;
    padding: 5px;
}
</style>                  