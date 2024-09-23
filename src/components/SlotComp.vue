<template>
    <slot v-for="x in foods" :foodName=x :key=x>

    </slot>
</template>

<script>
export default {
    data() {
        return {
            foods: ['Apple', 'Pizza', 'Rice', 'Fish', 'Cake']
        }
    }
}
</script>

<style></style>  