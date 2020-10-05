<template>
<div class="container">
    
  <!-- Div with detail page -->
    <div class="detail-page" >
        <div class="name-show"><h1>{{shows[0].title ? shows[0].original_title : shows[0].name}}</h1></div>
        <div class="backdrop-div">
            <img v-if="shows[0].backdrop_path" class="backdrop-img" :src="urlImg + shows[0].backdrop_path" alt= 'Image not found'>
            <img v-if ="!shows[0].backdrop_path" src="../imgs/backdrop.png" class="backdrop-img" >
        <div class="gradient-div"><h4>{{shows[0].overview}}</h4></div>
        </div>
        <div class="details row text-left">
            <div class="col-12 d-flex genres">
                <div v-for="genre in shows[0].genres" >
                    {{genre.name}}
                </div>
            </div> 
        </div>
        <!-- Get networks if it is a serie -->
        <div class="networks" v-if="shows[0].networks">
            <div class="d-flex">
            <img  v-for="network in shows[0].networks" :src="urlImg +network.logo_path" alt="">
            </div>
        </div>
        <!-- Homepage-button -->
        <div v-if="shows[0].homepage" class="text-left homepage-btn">
            <a target="_blank" :href="shows[0].homepage">Visit homepage</a> 
        </div>
        <div class="row more-details">
            <div class="col-lg-4 div-calendar">
                        <i class="fas fa-calendar-alt"></i> {{shows[0].release_date ? (new Date(shows[0].release_date).getFullYear()) : (new Date(shows[0].first_air_date).getFullYear())}}
            </div>
            <div class="col-lg-4 language-div " v-if="shows[0].spoken_languages"> 
                <i class="fas fa-language"></i>
                    <span v-for ="language in shows[0].spoken_languages">
                        {{language.name}}    
                    </span>
            </div>
            <div class="col-lg-4 language-div" v-if="shows[0].origin_country"> 
                <i class="fas fa-flag"></i>{{shows[0].origin_country[0]}}
            </div>
            <div v-if="shows[0].runtime||shows[0].episode_run_time" class="col-lg-4 duration-div"> 
               <i class="fas fa-clock"></i>{{shows[0].runtime ||shows[0].episode_run_time[0]}} min
            </div>
             
        </div>
    </div>
    <!-- Similar shows row -->
    <div v-if="this.similarShows.length>0"  >
     <h5 class="similar-title">Similar {{isMovie ? "movies" : "series"}}</h5>
    <div  v-bind:style="{marginLeft:margin + 'px', width:(similarShows.length *120 + similarShows.length *20) + 'px'}" class="d-flex" v-if="this.similarShows.length>0" id="similar-shows"> 
        <div class="similar-details" v-on:click ="goToDetailPage(similar.id,!isMovie ? true : false),getSimilarShows(similar.id)" v-for="similar in similarShows">
            <img v-if="similar.poster_path" class="similar-img" :src="urlImg + similar.poster_path" alt="">
            <img v-if="!similar.poster_path" class="similar-img" src="../imgs/netflix.png" alt="">
            <h6>{{similar.original_title ? similar.title : similar.name}}</h6>
        </div>
    </div>
   <div class="angle-div text-center">
            <i id="left" v-on:click="handleArrowLeft()" class="fas fa-angle-left"></i>
            <i id="right" v-on:click="handleArrowRight()" class="fas fa-angle-right"></i>
    </div>
    </div>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import Movies from './Movies.vue'
import axios from 'axios';

@Component({
    name:"DetailedPage"
})
export default class DetailedPage extends Vue {
    apiKey: string = "0567971fd9aa85a3b7dcd6d28eeabd21";
    @Prop()'shows' : any;
    @Prop()"goToDetailPage" : any;
    @Prop()"sort" : boolean;
    public urlImg: String = "https://image.tmdb.org/t/p/original/";
    public similarShows :Array<object> = [];
    public isMovie:boolean = this.shows[0].original_title ? true : false;
    public margin:number = -10;

    getSimilarShows(id: number){
        let media:String = this.isMovie ? "movie" : "tv"
        axios.get(`https://api.themoviedb.org/3/${media}/${id}/similar?api_key=${this.apiKey}&language=en-US&page=1`)
        .then(response => this.similarShows =response.data.results)
        .catch(err=> console.log(err))
    }
    

    handleArrowRight() {
        this.margin -= innerWidth /2 ;
        if (this.margin <= -(this.similarShows.length*120)) {
            this.margin = -10;
        }
    }
    handleArrowLeft() {
        this.margin += innerWidth/2;
        if (this.margin>-10) {
            this.margin = -10;
        }
        console.log(this.margin)
    }

    created(){
        this.getSimilarShows(this.shows[0].id)
    }
    
    
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


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
}

#similar-shows div{
    margin-right:20px;
    width:110px;
    word-wrap: break-word;
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

/* Style for smaller screens */
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
    padding: 3px;
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
.similar-img{
    width:100px!important;
}

.networks div{
    border-radius: 10px;
    width:100%;
    text-align: left;
    justify-content: left;
    display: flex;
    align-items: center;
}
.networks img{
    width:20%;
    height:20%;
    background: white;
    border-radius:10px;
    padding: 5px;
    margin-right: 10px;

}

}

/* Style for larger screens */
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
}
.more-details div{
    border-bottom:0.2px solid red;
    padding-bottom:8px;
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

.networks div{
    border-radius: 10px;
    width:800px;
    text-align: left;
    justify-content: left;
    display: flex;
    align-items: center;
}
.networks img{
    width:10%;
    height:10%;
    background: white;
    border-radius:10px;
    padding: 10px;
    margin-right: 20px;
}

}
</style>