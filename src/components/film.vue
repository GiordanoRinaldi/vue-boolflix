<template>
<div class="film">
    <div class="titolo">{{info.title}}</div>
    <div class="titolo-originale">{{info.original_title}}</div>
    <div class="lingua">
        <img :src="`https://www.unknown.nu/flags/images/${info.original_language}-100`" alt="">
    </div>
    <div class="voto">{{info.vote_average}}</div>
</div>
</template>

<script>
export default {
    name: "Film",
    props: ["info"]
}
</script>

<style>

</style>