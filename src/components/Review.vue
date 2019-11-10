<template>
    <div class="container">
        <h4>Reviews</h4>


        <div class="review-form" v-if="movie">
            <h5>Add new review</h5>
            <form @submit.prevent="addReview">
                <div class="form-group">
                    <label for="">Review</label>
                    <textarea v-model="review.content" cols="30" rows="5"></textarea>
                </div>
                <div class="form-group">
                    <label for="">Name</label>
                    <input type="text" v-model="review.reviewer">
                </div>
                <button class="btn btn-primary" :disabled="!review.reviewer || !review.content" type="submit">Submit</button>
            </form>
        </div>


        <div class="review" v-for="(review, index) in reviews" :key="index">
            <p>{{review.content}}</p>
            <div class="row">
                <div class="col-7">
                    <h5>{{review.reviewer}}</h5>
                </div>
                <div class="col-5">
                    <h5 class="pull-right">{{review.time}}</h5>
                </div>
            </div>
        </div>
    </div>    
</template>

<script>
const MOCK_REVIEWS = [
    {
        movie_id: 7128,
        content: 'Great show! I loved every single since. Definitely a mush watch',
        reviewer: 'Jane Doe',
        time: new Date().toLocaleDateString()
    }
]
export default {
    name: 'reviews',
    data() {
        return {
            mockReviews: MOCK_REVIEWS,
            movie: null,
            review: {
                content: '',
                reviewer: ''
            }
        }
    },
    computed: {
        reviews() {
            return this.mockReviews.filter(review => {
                return review.movie_id === this.movie;
            });
        }
    },
    methods: {
        addReview() {
            if(!this.movie || !this.review.reviewer || !this.review.content) {
                return;
            }
            let review = {
                movie_id: this.movie,
                content: this.review.content,
                reviewer: this.review.reviewer,
                time: new Date().toLocaleDateString(),
            }
            this.mockReviews.unshift(review);
        }
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