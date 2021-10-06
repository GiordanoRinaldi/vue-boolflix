<template>
    <div class="films px-4">
        <div class="film d-flex ">
            <Film v-for="(film, index) in resultsFilm" :key="index" :info="film"/>
        </div>
        <div class="tv d-flex ">
            <Film v-for="(film, index) in resultsTv" :key="index" :info="film"/>
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


        }
    },
    
    
}
</script>

<style>

</style>