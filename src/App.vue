<template>
  <div id="app">
    <Header @search="searchSomething"/>
    <main>
      <Films :filmSearched="searched"/>
    </main>
  </div>
</template>

<script>
import Films from './components/films.vue'
import Header from './components/header.vue'


export default {
  name: 'App',
  components: {
    Header,
    Films,
  },
  data(){
    return{
      searched: "",
    }
  },
  methods: {
    searchSomething(filmSearched) {
      this.searched = filmSearched;
    },
  }
}
</script>

<style lang="scss">

</style>
