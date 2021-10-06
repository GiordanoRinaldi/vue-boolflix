<template>
<div class="film">
    <div class="titolo">{{info.title}}{{info.name}}</div>
    <div class="titolo-originale">{{info.original_title}}{{info.original_name}}</div>
    <div class="lingua">
        <img :src="`https://www.unknown.nu/flags/images/${info.original_language}-100`" alt="">
    </div>
    <div class="voto">
        <div class="stella-piena">
            <span v-for="index in changeInStars(info.vote_average)" :key="index" ><i class="fas fa-star"></i></span>   
        </div>
        <div class="stella-vuota">
            <span v-for="index in 5 - changeInStars(info.vote_average)" :key="index" ><i class="far fa-star"></i></span>    
        </div>
    </div>
    <div class="poster">
        <img :src="`https://image.tmdb.org/t/p/w342${info.poster_path}`" alt=""  v-if="info.poster_path != null ">
    </div>
</div>
</template>

<script>
export default {
    name: "Film",
    props: ["info"],
    methods: {
        changeInStars(num){
            return Math.ceil(num / 2)
        }
    }
}
</script>

<style lang="scss" scoped>

</style>