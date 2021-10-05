<template>
    <header>
        <form>
            <input type="text" v-model="searchSomething">
            <button @click.prevent="$emit('search', searchSomething)">Cerca</button>
        </form>
        
    </header>
</template>

<script>
export default {
    name: "Header",
    data() {
        return{
            searchSomething: "",    
        }
    }
    
}
</script>

<style>

</style>