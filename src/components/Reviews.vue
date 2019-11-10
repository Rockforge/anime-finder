<template>
    <div class="container">
        <h4>Reviews</h4>

        <div class="review-form" v-if="anime">
            <h5>Add new review</h5>
            <form @submit.prevent="addReview">
                <div class="form-group">
                    <label for="">Review</label>
                    <textarea class="form-control" v-model="review.content" cols="30" rows="5"></textarea>
                </div>
                <div class="form-group">
                    <label for="">Name</label>
                    <input class="form-control" type="text" v-model="review.reviewer">
                </div>
                <button class="btn btn-primary" :disabled="!review.reviewer || !review.content" type="submit">Submit</button>
            </form>
        </div>


        <div class="review" v-for="(review, index) in reviews" :key="index">
            <p>{{review.content | truncate}}</p>
            <div class="row">
                <div class="col-7">
                    <h5>{{review.reviewer.username}}</h5>
                </div>
                <div class="col-5">
                    <h5 class="pull-right">{{review.date | parseDate}}</h5>
                </div>
            </div>
        </div>
    </div>    
</template>

<script>
import moment from 'moment';
import bus from '../bus';
export default {
    name: 'reviews',
    created() {
        bus.$on('new_anime', this.getReviews)
    },
    data() {
        return {
            reviews: [],
            anime: null,
            review: {
                content: '',
                reviewer: ''
            }
        }
    },
    filters: {
        parseDate: function(date) {
            return moment(date).format('YYYY-MM-D, h:mm:ss a');
        },
        truncate: function(content) {
            if (content.length > 50) {
                return content.substring(0,500) + '...';
            }
            return content;
        }
    },
    methods: {
        getReviews(anime) {
            let url = `https://api.jikan.moe/v3/anime/${anime.mal_id}/reviews`;
            window.console.log(url);
            fetch(url)
                .then(response => response.json())
                .then(data => {
                    window.console.log(data.reviews);
                    this.reviews = data.reviews; // Array
                });
        },
    }

}
</script>


<style scoped>
.container {
    padding: 0 2px;
}

.review {
    border: 1px solid #ddd;
    font-size: 0.95em;
    padding: 10px;
    margin: 15px 0 5px 0;
}

.review h5 {
    text-transform: uppercase;
    font-weight: bolder;
    font-size: 0.7em;
}

.pull-right {
    float: right;
}
.review-form {
    margin-top: 30px;
    border-top: 1px solid #ddd;
    padding: 15px 0 0 0;
}

</style>