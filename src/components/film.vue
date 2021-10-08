<template>
<div class="film-tv mb-4 me-4">
    <div class="info px-3 py-5">
        <div class="titolo"><strong>Titolo:</strong> {{info.title}}{{info.name}}</div>
        <div class="titolo-originale"><strong>Titolo originale:</strong> {{info.original_title}}{{info.original_name}}</div>
        <div class="lingua">
            <strong>Lingua:</strong> <img :src="`https://www.unknown.nu/flags/images/${info.original_language}-100`" alt="">
        </div>
        <div class="voto d-flex">
            <div class="stella-piena">
                <span v-for="index in changeInStars(info.vote_average)" :key="index" ><i class="fas fa-star"></i></span>   
            </div>
            <div class="stella-vuota">
                <span v-for="index in 5 - changeInStars(info.vote_average)" :key="index" ><i class="far fa-star"></i></span>    
            </div>
        </div> 
        <div class="overview"> 
            <div v-if="info.overview != false">
                <strong>Descrizione:</strong> {{info.overview}}
                <br>
                <strong v-if="listActor.length > 0" >Cast: </strong><span v-for="(list, index) in 5" :key="index">{{listActor[index]}} </span>
                 
            </div> 
            <span v-else> Descrizione non disponibile</span> 
        </div>   
        
    </div>
    
    <div class="poster">
        <img class="img-poster" :src="`https://image.tmdb.org/t/p/w342${info.poster_path}`" alt=""  v-if="info.poster_path != null ">
        <img class="img-not-f" src="https://www.themoviedb.org/assets/2/v4/glyphicons/basic/glyphicons-basic-38-picture-grey-c2ebdbb057f2a7614185931650f8cee23fa137b93812ccb132b9df511df1cfac.svg" alt="" v-else>
    </div>
</div>
</template>

<script>

import axios from 'axios'

export default {
    name: "Film",
    props: ["info"],
    data() {
        return{
            listActor: []
        }
    },
    methods: {
        changeInStars(num){
            return Math.ceil(num / 2)
        },
        generateActor(){
            axios.get(`https://api.themoviedb.org/3/movie/${this.info.id}/credits?`, {
                params: {
                    api_key: '75896a3c3b48da89500c9f72185c3497',
                }
            })
            .then((response) => {
                this.listActor=[];
                for (var i = 0; i < response.data.cast.length ; i ++ ) {
                    this.listActor.push(response.data.cast[i].name + ',')
                }
            }) 
        }
    },
    watch: {
        info : {
            handler: function(){
                this.generateActor()
            }
        }
    },
    created(){
        this.generateActor()
    }
}
</script>

<style lang="scss" scoped>
    .film-tv{
        position: relative;
        margin-top: 10px;
        margin-left: 5px;
        .info{
            position: absolute;
            opacity: 0;
            background-color: rgba(0, 0, 0, 0.8);
            width: 100%;
            border-radius: 15px;
            overflow: hidden;
            &:hover{
            opacity: 1;
            }
        }
        .titolo, .titolo-originale {
            height: 50px;
        }
        .lingua, .voto{
            height: 30px;
        }
        .poster {
            width: 342px;
            height: 513px;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0px 0px 15px -2px rgba(255,255,255,0.5);
            img{
                width: 100%;
                height: 513px;
            }
        }
        .overview{
            height: 257px;
            overflow-y: auto;
        }
        .lingua {
            img{
                width: 25px;
                height: 15px; 
            }
            

        }
    }

</style>