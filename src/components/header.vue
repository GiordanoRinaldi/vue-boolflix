<template>
    <header class="d-flex justify-content-between p-4">
        <div class="logo" @click="reload()">
            <img src="https://image.tmdb.org/t/p/original/wwemzKWzjKYJFfCeiB57q3r4Bcm.svg" alt="">
        </div>
        <form class="d-flex">
            <input type="text" v-model="searchSomething" placeholder="Crerca Film o Serie TV" class="form-control ">
            <button
            class=" btn btn-light ms-3"  
            @click.prevent="$emit('search', searchSomething)">
            Cerca
            </button>
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
    },
    methods: {
        reload() {
            location.reload()
        }
    }
    
}
</script>

<style lang="scss" scoped>
    .logo{
        cursor: pointer;
    }
    img {
        height: 40px;
    }
</style>