<template>
    <div class="films">
        <Film v-for="(film, index) in resultsFilms" :key="index" :info="film"/>
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
        resultsFilms: [],
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
            axios.get('https://api.themoviedb.org/3/search/movie', {
                params: {
                api_key: '75896a3c3b48da89500c9f72185c3497',
                query: this.filmSearched,
                language: 'it-IT',
                }
            })
            .then( (response) => {
                this.resultsFilms = response.data.results;
            });
        }
    },
    
    
}
</script>

<style>

</style>