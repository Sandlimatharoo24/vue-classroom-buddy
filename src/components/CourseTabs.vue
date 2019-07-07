<template>
    <div>
        <span class="tab" v-for="(tab, index) in tabs" :key="index" @click="selectTab(tab)"
        :class="{activeTab: selectedTab ===tab}">
        {{tab}}
        </span>

        <hr class="tab-separator">

        <div class="reviews-section" v-show="selectedTab ==='Reviews'">
            <p v-if="!reviews.length">There are no reviews yet!</p>
            <ol v-else>
                <li v-for="review in reviews" :key=review.id>
                    {{review.name}}: {{review.message}}({{review.rating}}/5)
                </li>
            </ol>
        </div>

        <div v-show="selectedTab === 'Give review'">
            <CourseReview @message-submitted="addMessage"/>
        </div>
    </div>
</template>

<script>
import CourseReview from '../components/CourseReview.vue'

export default {
  components: { CourseReview },
  data () {
    return {
      selectedTab: 'Reviews',
      reviews: [],
      tabs: ['Reviews', 'Give review']
    }
  },
  methods: {
    addMessage (CourseReview) {
      this.reviews.push(CourseReview)
      let sumRating = 0
      for (var i = 0; i < this.reviews.length; i++) {
        sumRating += this.reviews[i].rating
      }
      let avgRating = sumRating / this.reviews.length
      this.$emit('get-avg-feedback', avgRating)
    },
    selectTab (tab) {
      this.selectedTab = tab
    }
  }
}
</script>

<style>

</style>
