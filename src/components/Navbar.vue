<template>
<div class="container">
    <nav class="navbar row">
        <div class=" brand col-6 text-left"><img class="img-brand" src="../imgs/ymf-brand.png" alt=""></div>
        <div class="nav-items d-flex col-6 justify-content-end">
            <div class="nav-item"><a href=""><i class="fas fa-home"></i></a></div>
            <div class="nav-item"><a href=""><i class="fas fa-video"></i></a></div>
            <div class="nav-item"><a href=""><i class="fas fa-random"></i></a></div>
        </div>
    </nav>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import App from "../App.vue"
import axios from "axios";

@Component
export default class Navbar extends Vue {



}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

@media(max-width:600px){
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

}

.container{
    color: white;

}
.navbar a{
    text-decoration: none;
    color: white;
    text-transform: uppercase;
}
.nav-item{
    border:0.2px solid transparent;
    margin-left: 30px;
    border-radius: 5px;
    padding:10px;
    transition: all 0.5s ease;
    font-size: 20px;
    display: flex;
    align-content: center;
}

.nav-item span{ 
    margin-left:100px;
    transition: all 0.3s ease;
}
.nav-item:hover{
     border:0.2px solid rgb(255, 0, 0);
     transform: scale(1.2);
}

.brand h1{
    font-size:30px;
}
.img-brand{
    width:200px;
}



</style>