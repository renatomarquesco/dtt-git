<template>
<div class="container-fluid sticky-top">
  <div class="container">
    <!-- Navbar for arge screens -->
    <nav class="navbar visible-big row">
        <div class=" brand col-6 text-left"><img class="img-brand" src="../imgs/ymf-brand.png" alt=""></div>
        <div class="nav-items d-flex col-6 justify-content-end">
            <div v-on:click="reloadPage()" class="nav-item-big"><i class="fas fa-home"></i><span>Home</span></div>
            <div v-on:click="showNetworkLarge" class="nav-item-big"><i id="img-network-large" class="fas fa-video"></i><span id="span-net">Networks</span></div>
            <div v-on:click="getRandom(), closeNavbar()" class="nav-item-big"><i class="fas fa-random"></i><span>Random</span></div>
        </div>
    </nav>
    <!-- Network options in navbar for larger devices -->
    <div :style="{height:heightNetworkLarge + 'px'}" class="visible-big network-row-large row text-center flex-row-reverse">
        <div class="network-options-large justify-content-center" :style="{marginRight:marginRightNetwork + 'px'}" >
            <ul v-on:click="closeNavbar()">
                <li v-on:click="emitNetworkWithId(213)" >Netflix</li>
                <li v-on:click="emitNetworkWithId(1024)">Amazon Prime</li>
                <li v-on:click="emitNetworkWithId(1129)">HBO</li>
            </ul>
        </div>
    </div>
</div>
    <!-- Navbar for smaller devices -->
    <nav class="navbar visible-mobile nav-bar-sm">
        <div class="nav-brand  text-left">
            <img class="img-brand"  src="../imgs/ymf-brand.png" alt="">
        </div>
        <div class="burger-btn">
            <div id="burger-btn" v-on:click="openNavbar" class="nav-item"><i id="img-burger-btn" class="fas fa-bars"></i></div>
        </div>
    </nav>
    <!-- Network options for smaller devices -->
    <div :style="{height: this.heightDiv + 'px'}" class="row navbar-sm-row visible-mobile" >
    <div class="col-7 display-div-net text-center">
        <div :style="{height: this.heightNetwork + 'px'}" class="networks-options-navbar-sm">
            <ul v-on:click="closeNavbar()">
                <li v-on:click="emitNetworkWithId(213)" >Netflix</li>
                <li v-on:click="emitNetworkWithId(1024)">Amazon Prime</li>
                <li v-on:click="emitNetworkWithId(49)">HBO</li>
            </ul>
        </div>
    </div>
    <div class="navbar-options text-right col-5">
            <div class="home-sm" v-on:click ='reloadPage()' ><i class="fas fa-home"></i><span>Home</span></div>
            <div id="network-sm" v-on:click="showNetwork" class="net-sm"><i class="fas fa-video"></i><span>Networks</span></div>
            <div v-on:click="getRandom(),closeNavbar()" class="random-sm"><i class="fas fa-random"></i><span>Random</span></div>
    </div>
</div>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import Movies from "./Movies.vue"
import App from "../App.vue"
import axios from "axios";
import RandomMovie from "./RandomMovie.vue";


@Component({
    name:'Navbar',
    components:{
        RandomMovie,
    }
})
export default class Navbar extends Vue{
@Prop()"isThereRandom" : boolean;
@Prop()"shows" : Array<object>;
heightDiv : number = 0;
heightNetwork : number = 0;
marginRightNetwork : number = -250;
heightNetworkLarge : number = 0;

// Emitting the events to App.vue
getRandom() {
    this.$emit('get-random-movie')
}

emitNetworkWithId(idNetwork :number) {
    this.$emit("emit-network", idNetwork)
}

// Funtion to add style to Navbar (open and close)
openNavbar() {
    if (this.heightDiv ==0) {
        this.heightDiv = 180;
        (<HTMLElement>document.getElementById("img-burger-btn")).classList.add("fa-times");
        (<HTMLElement>document.getElementById("img-burger-btn")).classList.remove("fa-bars");

    }

    else {
        this.heightDiv = 0;
        (<HTMLElement>document.getElementById("img-burger-btn")).classList.remove("fa-times");
        (<HTMLElement>document.getElementById("img-burger-btn")).classList.add("fa-bars");
    }
}

closeNavbar() {
    this.heightDiv = 0;
    this.heightNetwork= 0;
    this.marginRightNetwork= -250;
    this.heightNetworkLarge= 0;
    (<HTMLElement>document.getElementById("img-burger-btn")).classList.remove("fa-times");
    (<HTMLElement>document.getElementById("img-burger-btn")).classList.add("fa-bars")
}

// Function to show Network options on different devices
showNetwork(){
      if (this.heightNetwork == 0) {
        this.heightNetwork = 130;

    }

    else {
        this.heightNetwork = 0;
    }
}

showNetworkLarge() {
    if (this.marginRightNetwork == -250) {
        this.marginRightNetwork = 40;
        this.heightNetworkLarge = 120;
    }

    else {
        this.marginRightNetwork = -250;
        this.heightNetworkLarge = 0;
    }
}

// Reloading page
reloadPage() {
    window.location.reload();
}

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container-fluid{
    background: #000000;  /* fallback for old browsers */
    background: -webkit-linear-gradient(to right, #434343, #000000);  /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(to left, #333333, #000000); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */;
    color: white;
}

/* Smaller devices */
@media(max-width:992px){
    .nav-item{
        padding:0!important;
       
    }
    .brand{

        margin-top: 0;
    }

    .img-brand{
        content: url("../imgs/ymf-mobile.png");
        width:100px!important
    }
    .visible-big{
        display: none;
    }

    .nav-bar-sm{
        display: flex;
        align-items: center;
        cursor: pointer;
        font-size: 30px;
    }

    .navbar-sm-row{
        overflow: hidden;
        transition: all 0.3s ease;
        
    }


    .navbar-options div{
        margin-bottom: 20px;
        border-bottom:0.2px solid red;
        padding: 3px;
        font-size: 18px;
    }

    .navbar-options i{
        margin-right: 10px;
        font-size: 18px;
    }

    .networks-options-navbar-sm{
        overflow: hidden;
        text-align: center;
        padding: 5px;
        transition: all 0.4s ease;
    }

    .networks-options-navbar-sm ul li{
        list-style: none;
        padding: 5px;
        font-size: 18px;
    }

    .display-div-net{
        display: flex;
        align-items: center;
    }

    #burger-btn{
        transition: all 0.4s ease;
    }
}

/* Larger devices */
@media(min-width:991.99px){
    .visible-mobile{
        display: none;
    }
}

.container{
    color: white;
    font-family: 'Dosis', sans-serif;
    overflow: hidden;

}

.navbar{
  background: transparent;
}
.navbar a{
    text-decoration: none;
    color: white;
    text-transform: uppercase;
}
.nav-item-big{
    border:0.2px solid transparent;
    margin-left: 30px;
    border-radius: 5px;
    width:50px;
    padding:10px;
    transition: all 0.5s ease;
    font-size: 20px;
    display: flex;
    align-items: center;
    overflow: hidden;
    cursor: pointer;
}

.nav-item-big i{
    cursor: pointer;
}
.nav-item-big span{ 
    transition: all 0.4s ease;
    margin-left: 70px;
}
.nav-item-big:hover{
    border:0.2px solid rgb(255, 0, 0);
    transform: scale(1.2);
    width:125px;
}

.nav-item-big:hover span{
    margin-left:10px;
}

.brand h1{
    font-size:30px;
}
.img-brand{
    width:200px;
}
.network-row-large ul li{
    list-style: none;
    border-bottom: 0.2px solid red;
    font-size: 18px;
    padding-bottom: 5px;
    padding-right: 5px;
    width:200px;
    cursor: pointer;
    transition: 0.4s all ease;
    
}
.network-row-large ul li:hover{
    padding-right: 20px;
    transform:scale(1.05)
    
}
.network-row-large{
    overflow: hidden;
    transition: all 0.2s ease;
}
.network-options-large{
    transition: all 0.4s ease;
    transition-delay: 0.4s;
}

</style>