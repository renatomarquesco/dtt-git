<template>
<div>
<div class="container body">
    <!-- Loading gif -->
    <div class="loading-img" v-if="movies.length == 0">
        <img src="../imgs/ajax-loader.gif" alt="">
    </div>
    <div class="movie row">

    <!--After getting API response  -->
    <div v-if="movies.length>1">
     <div v-if="searchSeries" class="row">
        <div class="col-md-3 col-6"  v-for="movie in movies" :key="movie.id" v-if="movie.poster_path">
            <img v-on:click= goToDetailPage(movie.id,true) class="img_poster" :src=" urlImg + movie.poster_path" alt="">
            <h5 >{{movie.name}}</h5>
            <h6>{{(new Date(movie.first_air_date).getFullYear())}}</h6>
        </div>
    </div>
    <div v-if="!searchSeries" class="row">
        <div class="col-md-3 col-6 movie-div" v-for="movie in movies" :key='movie.id' v-if="movie.poster_path">
            <img v-on:click="goToDetailPage(movie.id,false),$emit('no-sort')" class="img_poster" :src=" urlImg + movie.poster_path" alt="">
            <h5 >{{movie.title}}</h5>
            <h6>{{(new Date(movie.release_date).getFullYear())}}</h6>
        </div>
    </div>
  </div>
  </div>
  </div>
  <!-- Div with detail page -->
  <DetailedPage v-if="movies.length ==1" v-bind:goToDetailPage="goToDetailPage" v-bind:movies="this.movies"/>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import axios from "axios";
import Navbar from './Navbar.vue'
import Search from './Search.vue';
import DetailedPage from "./DetailedPage.vue"
@Component({
  name: 'Movies',
  components: {
    Search,
    DetailedPage,
  }
})
export default class Movies extends Vue {
    @Prop()"getShowsByNetwork":any;
    @Prop()"movies":Array<object>
    @Prop()"searchSeries":boolean;
    @Prop()"recommended":boolean
    apiKey: string = "0567971fd9aa85a3b7dcd6d28eeabd21";
    urlImg: String = "https://image.tmdb.org/t/p/original/"
    //Create array to store the movie data
    heightSortBox:number = 22;
    heightSortRow:number = 30;

    goToDetailPage(id:number, series:boolean){
    let resource : string = series ? "tv" : "movie";
    axios.get(`https://api.themoviedb.org/3/${resource}/${id}?api_key=${this.apiKey}`)
    .then(response => this.movies = [response.data])
    .catch(err=> console.log(err))
}

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@media(max-width:992px){
    .img_poster{
        width:130px!important;
    }
}
.loading-img{
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}
.movie img{
    cursor: pointer;
    border-radius:10px;
    margin-bottom: 10px;
    margin-top: 10px;
    transform: scale(0.9);
    transition: all 0.3s ease;
    width:200px;
}
.movie img:hover{
    transform: scale(1);
}
.toggle-button{
    height:24px;
    width:50px;
    background: rgb(255, 255, 255);
    border-radius:30px;
    margin-left:30px;
    margin-right:30px;
    padding:1.1px 1.2px;
    cursor: pointer;
    margin-top: 5px;
    transition: all 0.3s ease;
}
.diff-bg{
   background-color:rgb(255, 0, 0) ;
}
.inner-circle{
    background:rgb(255, 0, 0);
    height:22px;
    width:22px;
    border-radius:50%;
    transition:all 0.3s ease;
}
.series{
    margin-left:25.2px;
    background: white;
}
.switch{
    margin-top: 50px;
    color: rgb(218, 218, 218);
    font-size: 20px;
}
.movie{
    color:white;
    padding:20px;
}
.movie-div{
    transition: all 0.3s ease;
}
.input{
    background: transparent;
    color: white;
    border:none;
    border-bottom: 0.2px solid white;
    margin-top: 40px;
}
.input:focus{
    outline: none;
}

.search-btn{
    cursor: pointer;
    width:24px;
}
label{
    font-size: 20px;
}

</style>