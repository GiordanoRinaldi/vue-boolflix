<template>
    <div class="films mx-4">
        <div class="select-film">
            <select name="" id="">
                <option value="">Filtra per genere</option>
                <option v-for="(genre, index) in listGenreMovies.length" :key="index" value="">{{genre[index]}}</option>
            </select>
        </div>
        <div class="container-film">
            <h2 v-if="resultsFilm != false ">Film</h2>
            <div class="film" id="film">
                <div class="d-flex flex-nowrap">
                    <Film v-for="(film, index) in resultsFilm" :key="index" :info="film" :list="listGenreMovies"/>
                </div>
            </div>
            <button v-if="resultsFilm != false " class="btn scroll-right" @click="scrollRight('film')"><i class="fas fa-arrow-left"></i></button>
            <button v-if="resultsFilm != false " class="btn scroll-left" @click="scrollLeft('film')"><i class="fas fa-arrow-right"></i></button>
        </div>
        <div class="container-tv">
            <h2 v-if="resultsTv != false ">Serie TV</h2>
            <div class="tv" id="tv">
                <div class="d-flex flex-nowrap">
                    <Tv v-for="( tv, index) in resultsTv" :key="index" :info="tv" :list="listGenreTVs"/>
                </div>
            </div>
            <button v-if="resultsTv != false " class="btn scroll-right" @click="scrollRight('tv')"><i class="fas fa-arrow-left"></i></button>
            <button v-if="resultsTv != false " class="btn scroll-left" @click="scrollLeft('tv')"><i class="fas fa-arrow-right"></i></button>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import Film from "./film.vue"
import Tv from "./tv.vue"



export default {
    name: "Films",
    components: {
        Film,
        Tv,
    },
    props: ["filmSearched"], 
    data() {
        return{
        resultsFilm: [],
        resultsTv: [],
        listGenreMovies: [],
        listGenreMoviesOnlyName:[],
        listGenreTVs: [],

        }
    },
    watch: {
        filmSearched: {
            handler: function() {
                this.genetateFilmTv();
            },
        },
        // resultsFilm: {
        //     handler: function() {
        //         this.generateactorsFilm();
        //     }
        // },
        // resultsTv: {
        //     handler: function() {
        //         this.generateactorsTv();
        //     }
        // },

    },
    methods: {
        genetateFilmTv() {
            // parte per cercare film e serie 
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

                const genreListFilm = [];
                this.resultsFilm.forEach(
                    (elm) => {
                        elm.genre_ids.forEach(
                            (elm) => {
                                if (genreListFilm.includes(elm)){
                                genreListFilm.push(elm)
                                }
                            }
                        )
                    }
                );

                const genreListTV = [];
                this.resultsTv.forEach(
                    (elm) => {
                        elm.genre_ids.forEach(
                            (elm) => {
                                if (genreListTV.includes(elm)){
                                genreListTV.push(elm)
                                }
                            }
                        )
                    }
                );

                
                axios.get('https://api.themoviedb.org/3/genre/movie/list?api_key=75896a3c3b48da89500c9f72185c3497&language=it-IT')
                .then((response) => {
                    this.listGenreMovies = response.data.genres
                });

                axios.get('https://api.themoviedb.org/3/genre/tv/list?api_key=75896a3c3b48da89500c9f72185c3497&language=it-IT')
                .then((response) => {
                    this.listGenreTVs = response.data.genres
                })
                
            })
            

            // fine parte per cercare film serie 
        },

        


        // generateactorsFilm() {
        //     this.resultsFilm.forEach(elm => {
        //         let attori = []
        //         axios.get(`https://api.themoviedb.org/3/movie/${elm.id}/credits?`, {
        //             params: {
        //                 api_key: '75896a3c3b48da89500c9f72185c3497',
        //                 language: 'it-IT' 
        //             }
        //         })
        //         .then((response) => {
        //             for (var i = 0; i < 5; i ++ ) {
        //                 attori.push(response.data.cast[i].name)
        //             }
        //         })
        //         elm.actor = attori
        //     });
        // },

        // generateactorsTv() {
        //     this.resultsTv.forEach(elm => {
        //         let attori = []
        //         axios.get(`https://api.themoviedb.org/3/tv/${elm.id}/credits?`, {
        //             params: {
        //                 api_key: '75896a3c3b48da89500c9f72185c3497',
        //                 language: 'it-IT' 
        //             }
        //         })
        //         .then((response) => {
                     
        //             for (var i = 0; i < response.data.cast.length; i ++ ) {
        //                 attori.push(response.data.cast[i].name)
        //             }
        //         })

        //         elm.actor = attori
        //     });
        // },

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
    }
    .container-film, .container-tv {
        position: relative;
        .scroll-left, .scroll-right{
            position: absolute;
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