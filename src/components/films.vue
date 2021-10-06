<template>
    <div class="films mx-4">
        <h2 v-if="resultsFilm != false ">Film</h2>
        <div class="film" id="film">
            <div class="d-flex flex-nowrap">
                <Film v-for="(film, index) in resultsFilm" :key="index" :info="film"/>
            </div>
            <button v-if="resultsFilm != false " class="btn scroll-right" @click="scrollRight('film')"><i class="fas fa-arrow-left"></i></button>
            <button v-if="resultsFilm != false " class="btn scroll-left" @click="scrollLeft('film')"><i class="fas fa-arrow-right"></i></button>
        </div>
        <h2 v-if="resultsTv != false ">Serie TV</h2>
        <div class="tv" id="tv">
            <div class="d-flex flex-nowrap">
                <Film v-for="(film, index) in resultsTv" :key="index" :info="film"/>
            </div>
            <button v-if="resultsTv != false " class="btn scroll-right" @click="scrollRight('tv')"><i class="fas fa-arrow-left"></i></button>
            <button v-if="resultsTv != false " class="btn scroll-left" @click="scrollLeft('tv')"><i class="fas fa-arrow-right"></i></button>
        </div>
        
        
        
        
    </div>
</template>

<script>
import axios from 'axios'
import Film from "./film.vue"



export default {
    name: "Films",
    components: {
        Film,
    },
    props: ["filmSearched"], 
    data() {
        return{
        resultsFilm: [],
        resultsTv: [],
        }
    },
    watch: {
        filmSearched: {
            handler: function() {
                this.genetateTotal()
            },
        }
    },
    methods: {
        genetateTotal() {
            const film  = axios.get('https://api.themoviedb.org/3/search/movie', {
                params: {
                api_key: '75896a3c3b48da89500c9f72185c3497',
                query: this.filmSearched,
                language: 'it-IT',
                }
            })

            const tv  = axios.get('https://api.themoviedb.org/3/search/tv', {
                params: {
                api_key: '75896a3c3b48da89500c9f72185c3497',
                query: this.filmSearched,
                language: 'it-IT',
                }
            })

            axios.all([film,tv])
            .then( (response) => {

                this.resultsFilm = response[0].data.results;

                this.resultsTv = response[1].data.results;


            });
        },

        scrollLeft(obj){
           let scrollAmount = 0;
           let slider = setInterval(function(){
               document.getElementById(obj).scrollLeft += 50;
               scrollAmount += 50;
               if(scrollAmount >= 390) {
                   window.clearInterval(slider)
               }
           }, 20);
        },

        scrollRight(obj){
            let scrollAmount = 0;
            let slider = setInterval(function(){
                document.getElementById(obj).scrollLeft -= 50;
                scrollAmount += 50;
                if(scrollAmount >= 390) {
                    window.clearInterval(slider)
                }
            }, 20);
        }
    },
    
    
}
</script>

<style lang="scss" scoped>
.films {
    height: calc(100vh - 88px); 
    overflow-y: auto;

    .film, .tv {
        position: relative;
        overflow-x: hidden;
        .scroll-left, .scroll-right{
            position: sticky;
            background-color: white;
            color: black;
            width: 50px;
            opacity: 0.4;
            &:hover{
                opacity: 1;
            }
        }
        .scroll-left{
            left: 94vw;
            bottom: 15vw;
        }
        .scroll-right{
            left: 0vw;
            bottom: 15vw;
        }
    }

    
}

</style>