<template>
  <div id="app">
    <Navbar
      v-on:get-random-movie="showRandom"
      v-on:emit-network="getShowsByNetwork"
    />
    <Search
      v-if="!this.isThereRandom"
      v-on:send-search-input="handleSearch"
      v-on:send-type-toggle="handleToggle"
    />
    <Sort
      v-if="sort"
      v-on:ascending-year="ascendingYear"
      v-on:descending-year="descendingYear"
    />
    <Movies
      v-if="!isThereRandom"
      v-bind:shows="this.shows"
      v-bind:searchSeries="this.searchSeries"
      v-bind:sort ="this.sort"
      v-on:no-sort="changeSort"
    />
    <RandomMovie
      v-if="isThereRandom"
      v-bind:showRandom="showRandom"
      v-bind:randomMovie="this.randomMovie"
    />
    <Footer
     v-bind:getShowByNetwork="getShowByNetwork"
     v-bind:showRandom="showRandom"
     v-on:emit-network="getShowsByNetwork"
    />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Movies from './components/Movies.vue';
import Navbar from './components/Navbar.vue';
import Footer from './components/Footer.vue';
import Search from './components/Search.vue'
import RandomMovie from './components/RandomMovie.vue';
import Sort from './components/Sort.vue';
import axios from 'axios';
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue';
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap-vue/dist/bootstrap-vue.css';
@Component({
  name: 'App',
  components: {
    Movies,
    Navbar,
    Search,
    Footer,
    RandomMovie,
    Sort,
  },
})
export default class App extends Vue {
public isThereRandom: boolean = false;
public apiKey: string = "0567971fd9aa85a3b7dcd6d28eeabd21";
public randomMovie: object = {}
// Create array to store response with movies and series collected by API.
public shows: any = []

public searchSeries: boolean = false
public search: string = ''
public recommended: boolean = true;
public sort:boolean = true;

 // Function to call API
public handleSearch(searchInput: string, type: string) {
        this.search = searchInput;
        this.searchSeries = type === "series";
        let apiUrl : string = this.buildApiUrl(this.search, this.searchSeries);
        let query : string = this.search === "" ? "" : `query=${this.search}&`
        axios.get(`${apiUrl}?${query}api_key=${this.apiKey}`)
            .then(response => this.shows = response.data.results)
            .catch(err => console.log(err))
        this.recommended = this.search === "";
        this.sort = true;
    }
// Function to build Api URL
public buildApiUrl(search : string, series: boolean) : string {
        let resource : string = series ? "tv" : "movie";
        let api : string = search === "" ? "trending" : "search";
        let resourceSubPath : string = search === "" ? "day" : "";
        return `https://api.themoviedb.org/3/${api}/${resource}/${resourceSubPath}`;
    }
// Function to switch toggle (series ans movies)
public handleToggle(type:string) {
        this.handleSearch(this.search, type);
  }

// Funtion to show the random movies and series

public showRandom() { 
  let randomPage:number = Math.floor(Math.random()*499)
  let randomID:number = Math.floor(Math.random()*999999)
  axios.get(`https://api.themoviedb.org/3/movie/${randomID}?api_key=${this.apiKey}&language=en-US&include_adult=false&include_video=false&page=${randomPage}`)
  .then((response) => !response.data.adult ? this.randomMovie = response.data : this.showRandom())
  .catch(err => this.showRandom())
  console.log(this.randomMovie); 
  this.isThereRandom = true;
  this.sort = false;
  }
  
// Function to get shows by Network (in navbar)
public getShowsByNetwork(idNetwork: number) { 
    this.searchSeries = true
    axios.get(`https://api.themoviedb.org/3/discover/tv?api_key=0567971fd9aa85a3b7dcd6d28eeabd21&language=en-US&with_networks=${idNetwork}`)
    .then(response => this.shows = response.data.results)
    .catch(err => console.log(err))
    this.isThereRandom = false;
    this.sort = true;
}

// Function to sort the array by year (ascending)
public ascendingYear() {
        if(this.searchSeries) {
            this.shows.sort(function (a:any , b:any){
            return (new Date(a.first_air_date).getFullYear()) - (new Date(b.first_air_date).getFullYear())
        })
        }

        else{
            this.shows.sort(function (a:any,b:any){
            return (new Date(a.release_date).getFullYear()) - (new Date(b.release_date).getFullYear())
        })
      }
    }

// Function to sort the array by year (descending)
public descendingYear() {
    if(this.searchSeries){
            this.shows.sort(function (a:any , b: any){
            return (new Date(b.first_air_date).getFullYear()) - (new Date(a.first_air_date).getFullYear())
      })
    }

    else{
      this.shows.sort(function (a: any,b: any){
      return (new Date(b.release_date).getFullYear()) - (new Date(a.release_date).getFullYear())
      })

    }
  }
// Function to change sort prop to false or true and allow the component to be showned on screen
public changeSort() {
  this.sort = false;
}

// Calling the recommened movies
created() {
      this.handleSearch(this.search, this.searchSeries ? "series" : "movies");
}

}
</script>

<style>
#app {
  font-family: "Dosis", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100%;
  padding-top: 50px;
  background: #000000; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to right,
    #434343,
    #000000
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to left,
    #333333,
    #000000
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}
</style>