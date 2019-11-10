<template>
    <div class="container">
        <div class="row mb-3">
           <div class="col-12">
                <form @submit.prevent="fetchAnime()">
                    <div class="form-row">
                        <div class="col-8">
                            <input type="text" class="form-control" v-model="title">
                        </div>
                        <div class="col-4">
                            <button class="btn btn-primary">Search Titles</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
        <div class="loader" v-if="loading">
            <img src="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/0.16.1/images/loader-large.gif" alt="loader">
        </div>
        <div v-else-if="error_message">
            <h3>{{error_message}}</h3>
        </div>
        <div class="row" v-else-if="Object.keys(anime).length !== 0" id="anime">
            <div class="col-12 mb-3">
                <h4>{{anime.title}}</h4>
            </div>
            <div class="row">
                <div class="col-6 text-center">
                    <img :src="anime.image_url" :alt="anime.title">
                </div>
                <div class="col-6">
                    <p>{{anime.synopsis}}</p>
                    <small><strong>Score: </strong>{{anime.score}}</small>
                </div>
            </div>
        </div>


    </div>
</template>

<script>
const API_URL = 'https://api.jikan.moe/v3/search/anime';
function buildUrl(title) {
    return `${API_URL}?q=${title}&limit=1`;
}
import bus from '../bus';
export default {
    name: 'anime',
    data() {
        return {
            title: '',
            error_message: '',
            loading: false,
            anime: {}
        }
    },
    methods: {
        fetchAnime() {
            let title = this.title;
            if(!title) {
                alert('Please enter a title to search for');
                return;
            }
            this.loading = true;
            fetch(buildUrl(title))
                .then(response => response.json())
                .then(data => {
                    window.console.log(data);
                    this.loading = false;
                    this.error_message = '';
                    bus.$emit('new_anime', data.results[0]);
                    if(data.errorcode) {
                        this.error_message = `Sorry, anime with title '${title}' not found.`;
                        return;
                    }
                    this.anime = data.results[0];
                }).catch((e) => {
                    window.console.log(e);
                });
        }
    }
    
}
</script>

<style scoped>
#movie {
    margin: 30px 0;
}
.loader {
    text-align: center;
}

</style>