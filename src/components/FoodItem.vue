<template>
<h1>
    {{ count }}
</h1>
<button v-on:click="incrementer">
    click
</button>
</template>
<script>
export default {
data(){
    return{
count:null
    }
},
methods:{
    incrementer(){
        if(this.count<10)
        this.count++
    else{
        this.count=0
    }
    }
}

}
</script>
<style>
</style>