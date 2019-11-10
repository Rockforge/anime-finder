<template>
    <div class="container">
        <div class="row">
            <form @submit.prevent="fetchMovie()">
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

        <div class="loader" v-if="loading">
            <img src="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/0.16.1/images/loader-large.gif" alt="loader">
        </div>
        <div v-else-if="error_message">
            <h3>{{error_message}}</h3>
        </div>

        <div class="row" v-else-if="Object.keys(movie).length !== 0" id="movie">
            <div class="col-7">
                <h4>{{movie.show_title}}</h4>
                <img :src="movie.poster" :alt="movie.show_title">
            </div>
            <div class="col-5">
                <p>{{movie.summary}}</p>
                <small><strong>Cast: </strong>{{movie.show_cast}}</small>
            </div>
        </div>


    </div>
</template>

<script>
const API_URL = 'https://netflixroulette.net/api/api.php';
function buildUrl(title) {
    return `${API_URL}?title=${title}`;
}
export default {
    name: 'movie',
    data() {
        return {
            title: '',
            error_message: '',
            loading: false,
            movie: {}
        }
    },
    methods: {
        fetchMovie() {
            let title = this.title;
            if(!title) {
                alert('Please enter a title to search for');
                return;
            }
            this.loading = true;
            fetch(buildUrl(title))
                .then(response => response.json())
                .then(data => {
                    this.loading = false;
                    this.error_message = '';
                    if(data.errorcode) {
                        this.error_message = `Sorry, movie with title '${title}' not found.`;
                        return;
                    }
                    this.movie = data;
                }).catch((e) => {
                    console.log(e);
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