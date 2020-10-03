<template>

    <div class="container-fluid">
        <footer>
            <a href="">Site Map</a>
        </footer>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import App from "../App.vue"
import axios from "axios";

@Component
export default class Footer extends Vue {



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
footer a{
    color:white;
    cursor: pointer;
}

</style>