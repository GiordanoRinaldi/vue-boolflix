<template>
    <div class="films">
        <Film v-for="(film, index) in resultsTotal" :key="index" :info="film"/>
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
        resultsTotal: [],
        }
    },
    watch: {
        filmSearched: {
            handler: function() {
                this.genetateFilms()
            },
        }
    },
    methods: {
        genetateFilms() {
            let film  = axios.get('https://api.themoviedb.org/3/search/movie', {
                params: {
                api_key: '75896a3c3b48da89500c9f72185c3497',
                query: this.filmSearched,
                language: 'it-IT',
                }
            })

            let tv  = axios.get('https://api.themoviedb.org/3/search/tv', {
                params: {
                api_key: '75896a3c3b48da89500c9f72185c3497',
                query: this.filmSearched,
                language: 'it-IT',
                }
            })

            axios.all([film,tv])
            .then( (response) => {
                let resultsFilms = response[0].data.results;
                let resultsTv = response[1].data.results;

                this.resultsTotal = resultsFilms.concat(resultsTv)

            });


        }
    },
    
    
}
</script>

<style>

</style>