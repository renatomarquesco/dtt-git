<template>
<div class="container">
    
  <!-- Div with detail page -->
    <div class="detail-page" >
        <div class="name-show"><h1>{{movies[0].original_title ? movies[0].original_title : movies[0].original_name}}</h1></div>
        <div class="backdrop-div"><img class="backdrop-img" :src="movies[0].backdrop_path ? urlImg + movies[0].backdrop_path :'../imgs/backdrop.png'" alt= 'Image not found'>
        <div class="gradient-div"><h4>{{movies[0].overview}}</h4></div>
        </div>
        <div class="details row">
            <div class="col-12 overview"></div>
            <div class="col-12 d-flex genres">
                <div v-for="genre in movies[0].genres" >
                    <h6 >{{genre.name}}
                    </h6>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import Movies from "./Movies.vue"
import axios from "axios";



@Component
export default class DetailedPage extends Vue {
    name:"DetailedPage"
    @Prop()"movies";
    urlImg: String = "https://image.tmdb.org/t/p/original/";
    backdrop: String = "../imgs/backdrop.png"

    created(){
        console.log(this.movies)
    }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

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
}.genres{
    padding-left: 10px;
}
.genres div{
    margin-right: 10px;
    border-radius:5px;
    background:white;
    color:rgb(36, 35, 35);
    display: flex;
    align-content: center;
    padding: 6px;
    margin-top: 20px;
    margin-bottom: 40px;
}

.genres div h6{
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

#img-to-click{
    cursor:pointer;
    display: flex;
    align-items: center;
    justify-content: end;
}


@media (max-width:500px){
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
    font-size: 24px!important;
}

}

@media(min-width:501px){
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


}
</style>