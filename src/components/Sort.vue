<template>
    <div class="container">
     <div :style="{height:heightSortRow +'px'}" class="sort row">
        <div class="col-lg-4 col-md-6 col-sm-7 col-8">
        <ul :style ="{height:heightSortBox + 'px', width:widthSortBox + 'px'}">
            <li v-on:click="openSortBox" class="li-title">Sort<i class="fas fa-caret-down"></i> </li>
            <li class="li-element" v-on:click="$emit('ascending-year'),closeSortBox()">Release date (Ascending)</li>
            <li class="li-element" v-on:click="$emit('descending-year'), closeSortBox()">Release date (Descending)</li>
        </ul>
        </div>
    </div>
    </div>
</template>
<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import axios from "axios";

@Component({
  name: 'Sort' 
})

export default class Movies extends Vue {
heightSortBox:number = 30;
heightSortRow:number = 35;
widthSortBox:number = 70;


openSortBox(){
    if(this.heightSortBox == 30){
            this.heightSortBox = 110
            this.heightSortRow = 110;
            this.widthSortBox = 250;
    }

    else{
            this.heightSortBox = 30;
            this.heightSortRow = 35;
            this.widthSortBox = 70;
    }
}

closeSortBox(){
    this.heightSortBox = 30;
    this.heightSortRow = 35;
    this.widthSortBox = 70;
}
}
</script>

<style scoped>
.container{
    padding-left:32px;
}
.sort{
    display: flex;
    text-align: right;
    overflow: hidden;
    transition: all 0.3s ease;
    padding-left: 10px;
    color: white;
    margin-top: 20px;
}

.sort div{
    display: flex;
    text-align: left;

}
.sort ul li{
    list-style: none;
    margin-right: 10px;
    margin-left:-30px;
    position: relative;
    border-radius:10px;
    transition: all 0.3s ease;
    cursor: pointer;

}
.sort ul{
    margin-left: 10px;
    margin-top: 0;
    border:0.2px solid rgb(126, 125, 125);
    border-radius: 10px;
    padding-top: 0;
    overflow: hidden;
    transition: all 0.3s ease;
}

.sort ul li i{
    position: absolute;
    right: 2%;
    margin-top: 4px;
}
.li-title{
    padding-left:5px;
    padding-right:5px;
    margin-bottom: 5px;
}
.li-element{
    padding: 5px 5px;
}
.li-element:hover{
    background: rgb(73, 72, 72);
}

@media(max-width:768px){
.li-element{
    font-size: 14px;
}
}
</style>