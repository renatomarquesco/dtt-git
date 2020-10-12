<template>
<div class="container">
    <div class="random-page" >
         <div class="name-show"><h1>{{randomMovie.title}}</h1></div>
        <div class="backdrop-div">
            <img v-if="randomMovie.backdrop_path" class="backdrop-img" :src="urlImg + randomMovie.backdrop_path" alt= 'Image not found'>
            <img v-if ="!randomMovie.backdrop_path" src="../imgs/backdrop.png" class="backdrop-img" >
        <div class="gradient-div"><h4>{{randomMovie.overview}}</h4></div>
        </div>
        <div class="details row text-left">
            <div class="col-12 d-flex genres">
                <div v-for="genre in randomMovie.genres" >
                    {{genre.name}}
                </div>
            </div> 
        </div>
    
        <!-- Homepage-button -->
        <div v-if="randomMovie.homepage" class="text-left homepage-btn">
            <a target="_blank" :href="randomMovie.homepage">Visit homepage</a> 
        </div>
        <!-- Row with more info -->
        <div class="row more-details">
            <div class="col-lg-4 div-calendar">
                        <i class="fas fa-calendar-alt"></i> {{new Date(randomMovie.release_date).getFullYear()}}
            </div>
        <!-- Row with language and conditions. If there is no spoken languages property than getting the country where show was produced -->
            <div class="col-lg-4 language-div " v-if="randomMovie.spoken_languages"> 
                <i class="fas fa-language"></i>
                    <span v-for ="language in randomMovie.spoken_languages">
                        {{language.name}}    
                    </span>
            </div>
             <div class="language-div col-lg-4 " v-if="randomMovie.origin_country"> 
                <i class="fas fa-flag"></i>{{randomMovie.origin_country[0]}}
            </div>
            <div v-if="randomMovie.runtime" class="col-lg-4 duration-div"> 
               <i class="fas fa-clock"></i>{{randomMovie.runtime}} min
            </div>
        </div>
    </div>
    <!-- Button to get another random movie -->
    <button class="random-btn" v-on:click="showRandom()"> I need another suggestion!</button>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import App from '../App.vue';
import Navbar from './Navbar.vue';
import axios from 'axios';

@Component({
    name: 'RandomMovie',
})

export default class RandomMovie extends Vue {
// Getting properties from APP.vue
@Prop()"showRandom" : any;
@Prop()"randomMovie" : object;

urlImg :string = "https://image.tmdb.org/t/p/original/";

}
</script>

<style scoped>
.container{
    color: white;
}
.name-show{
    margin-top: 35px;
}
.random-page img{
    width:100%;
}
.random-btn{
    text-decoration: none;
    width:200px;
    color:white;
    cursor: pointer;
    background: red;
    padding: 10px;
    font-size: 17px;
    border-radius:10px;
    margin-bottom: 20px;
    margin-top: 23px;
    transform: scale(0.9);
    transition: all 0.3s ease;
    border:0.2px solid white;
}

.random-btn:hover{
    transform: scale(1);
}
.container{
    overflow: hidden;
}

.detail-page img{
    width:100%;
}

.name-show{
    margin-bottom: 30px;
    color:rgb(255, 255, 255);
    font-family: 'Josefin Sans', sans-serif;
    text-transform: uppercase;
    font-size: 25px;
}

.detail-page{
    color:white
}

.genres{
    padding-left: 17px;
}
.genres div{
    margin-right: 10px;
    border-radius:5px;
    background:white;
    color:rgb(36, 35, 35);
    display: flex;
    align-items: center;
    padding: 5px;
    margin-top: 20px;
    margin-bottom: 40px;
    font-size: 18px;
    font-weight: 700;
}

.overview{
    text-align: justify;
    margin-top: 40px;
}
.backdrop-img{
    border-radius:10px;
}

.homepage-btn{
    text-decoration: none;
    color:white;
    cursor: pointer;
    background: red;
    padding: 10px;
    border-radius:10px;
    margin-bottom: 20px;
    transform: scale(0.9);
    transition: all 0.3s ease;
    width:115px;
    margin-left:-8px;
}
.homepage-btn a{
    color: white;
}

.homepage-btn:hover{
    transform: scale(1);
}

.networks {
    text-align: left;
    margin-bottom: 30px;
    display: flex;
    align-items: center;
}

.networks div{
    border-radius: 10px;
    width:100px;
    text-align: center;
    display: flex;
    align-items: center;
}
.networks img{
    margin-left:0px;
    width:70%;
    height:70%;
    margin-left: 0px;
}

.similar-img{
    width:120px;
    transform: scale(0.9);
    border-radius:10px;
    cursor: pointer;
    transition: all 0.3s ease;
}
.similar-img:hover{
    transform: scale(1);
}
.similar-title{
    color: white;
    margin-top: 30px;;
}
#similar-shows{
    overflow-x: hidden;
    color: white;
    transition: all 0.3s ease;
    margin-top: 20px;
    border:1px solid yellow;
}

#similar-shows div{
    margin-right:20px;
}
.angle-div{ 
    color: white;
    font-size:40px;
}

.angle-div i{
    padding-left: 20px;
    padding-right: 20px;
    cursor: pointer;
}

/* Smaller devices */
@media (max-width:991.99px){
.backdrop-div{
    position: relative;
}
.gradient-div{
    width:100%;
    height: auto;
    padding:3px;
    margin-top: 10px;
}
.gradient-div h4{
    margin-top: 0px!important;
    text-align: justify;
    font-size:19px;
}
.name-show h1{
    font-size: 26px!important;
}
.genres div{
    margin-right: 10px;
    border-radius:5px;
    background:white;
    color:rgb(36, 35, 35);
    display: flex;
    align-items: center;
    padding: 2px;
    margin-top: 20px;
    margin-bottom: 40px;
    font-size: 15px;
    font-weight: 700;
}

.more-details i{
    font-size: 24px;
    margin-right:10px;
}
.more-details{
    font-size:18px;
    border-radius:10px;
    padding:10px;
    display: flex;
    align-items: center;
    text-align: left;
    margin-top: 20px;
}

.more-details div{
    border-bottom:0.2px solid red;
    margin-top: 10px;
}

}
/* Larger devices */
@media(min-width:992px){
.backdrop-div{
    position: relative;
}
.gradient-div{
    height:auto;
    position: absolute;
    width:100%;
    bottom:0;
    border-radius:9px;
    border: none;
    padding: 25px;
    background: rgb(0,0,0);
    background: linear-gradient(0deg, rgba(0,0,0,0.9108018207282913) 85%, rgba(195,195,200,0) 100%, rgba(210,208,208,1) 100%);
}
.gradient-div h4{
    margin-top: 20px;
    text-align: justify;
}
.more-details i{
    font-size: 25px;
    margin-right:10px;
}
.more-details{
    font-size:20px;
    padding:10px;
    display: flex;
    align-items: center;
    align-content: center;
    text-align: left;
    margin-top: 30px;
    border-bottom:0.2px solid red;
}

h5{
    font-size: 30px;
}

.duration-div{
    text-align: right;
}

.language-div{
    text-align: center;
}


}
</style>

