<template>
    <div id="app">
        <div class="container">
            <div class="heading">
                <h2>Christian's Anime Finder</h2>
                <h6 class="subheader"> Realtime anime reviews with Vue.js and Pusher </h6>
            </div>


            <div class="row">
                <div class="col-7">
                    <anime></anime>
                </div>
                <div class="col-5">
                    <reviews></reviews>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Anime from './components/Anime.vue';
import Reviews from './components/Reviews.vue';
export default {
    name: 'app',
    components: {
        Anime, Reviews
    }
}
</script>

<style>
  #app .heading {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin: 60px 0 30px;
    border-bottom: 1px solid #eee;
  }
</style>