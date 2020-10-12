<template>

    <div class="container-fluid">
        <div class="site-map mt-3 text-left" :style="{height:this.heightSiteMap + 'px'}">
            <ul>
                <li v-on:click="reloadPage()">Home</li>
                <li>Networks
                    <ul>
                        <li v-on:click="emitNetworkWithId(213)" > Netflix</li>
                         <li v-on:click="emitNetworkWithId(1024)" > Amazon Prime</li>
                       <li v-on:click="emitNetworkWithId(49)" >HBO</li>
                    </ul>
                </li>
                <li v-on:click="showRandom">Random</li>
            </ul>
        </div>
        <footer>
            <span v-on:click="showSiteMap()">Site Map</span>
        </footer>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import App from "../App.vue"
import axios from "axios";

@Component
export default class Footer extends Vue {
@Prop()'getShowByNetwork' :any;
@Prop() 'showRandom' :any;
heightSiteMap:number = 0;

emitNetworkWithId(idNetwork :number) {
    this.$emit("emit-network", idNetwork)
}

showSiteMap(){
    if(this.heightSiteMap == 0){
    this.heightSiteMap = 150;
    }
    else{
        this.heightSiteMap = 0;
    }
}

reloadPage(){
    window.location.reload();
}

}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.container-fluid{
    margin-bottom: 0;
    border-top: 0.3px solid white;
    margin-top: 30px;
    z-index: -111;
}
footer{
    text-align: left;
    padding: 10px;
    height:50px;
}
footer span{
    color:white;
    cursor: pointer;
}
.site-map{
    color: white;
    overflow: hidden;
    transition: all 0.3s ease;
}
.site-map ul li{
    cursor: pointer;
}
</style>