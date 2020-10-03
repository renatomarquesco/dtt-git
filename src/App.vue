<template>
  <div id="app">
    <Navbar v-on:get-random-movie="showRandom" v-on:emit-network="getShowsByNetwork" />
    <Movies v-if="!isThereRandom" v-bind:movies="this.movies" v-bind:searchSeries="this.searchSeries"/>
    <RandomMovie v-if="isThereRandom" v-bind:showRandom ="showRandom" v-bind:randomMovie="this.randomMovie" />
    <Footer />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Movies from './components/Movies.vue';
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";
import RandomMovie from './components/RandomMovie.vue';
import axios from "axios";
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
@Component({
  name: 'App',
  components: {
    Movies,
    Navbar,
    Footer,
    RandomMovie,
  },
})
export default class App extends Vue {
isThereRandom:boolean= false;
apiKey: string = "0567971fd9aa85a3b7dcd6d28eeabd21";
randomMovie:object = {}
movies: Array<object> = []
searchSeries: boolean = false


showRandom(){ 
  let randomPage:number = Math.floor(Math.random()*499)
  let randomID:number = Math.floor(Math.random()*999999)
  axios.get(`https://api.themoviedb.org/3/movie/${randomID}?api_key=${this.apiKey}&language=en-US&include_adult=false&include_video=false&page=${randomPage}`)
  .then((response) => !response.data.adult ? this.randomMovie = response.data : this.showRandom())
  .catch(err=>this.showRandom())
  console.log(this.randomMovie); 
  this.isThereRandom = true;
  }
  


  getShowsByNetwork(idNetwork:number){ 
    this.searchSeries = true
    axios.get(`http://api.themoviedb.org/3/discover/tv?api_key=0567971fd9aa85a3b7dcd6d28eeabd21&language=en-US&with_networks=${idNetwork}`)
    .then(response=>this.movies = response.data.results
)
    .catch(err=> console.log(err))
    this.isThereRandom = false;
}

}
</script>

<style>
#app {
  font-family: 'Dosis', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100%;
  padding-top:50px;
  background: #000000;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #434343, #000000);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to left, #333333, #000000); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */;
  
}
</style>