<template>
<div class="container">
     <div class="title-recommend">
        <h3>Recommended for you</h3>
    </div>
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
export default class Movies extends Vue {}

</script>

<style scoped>
.title-recommend{
    padding: 20px;
    display: flex;
    color: white;
}
.title-recommend h3{
    font-size: 30px;
}
</style>