<template>
  <div class="space-y-4">
    <div class="flex items-center space-x-8">
      <div class="w-full">
        <label
          for="q"
          class="sr-only"
        >Search</label>
        <input
          id="q"
          v-model="search"
          type="text"
          class="w-full p-4 border border-black dark:border-white dark:bg-black dark:text-white"
          placeholder="Enter your search"
        >
      </div>
      <button
        type="submit"
        class="relative inline-flex items-center justify-center px-8 py-5 font-mono font-bold leading-none tracking-widest uppercase bg-white border border-black dark:bg-black dark:border-white"
      >
        Search
      </button>
    </div>

    <SearchResults
      v-if="searchedReviews.length > 0"
      :reviews="searchedReviews"
    />
  </div>
</template>

<static-query>
  query {
    reviews: allReview(sortBy: "title", order: ASC) {
      edges {
        node {
          title
          path
          date
        }
      }
    }
  }
</static-query>

<script>
import SearchResults from '@/components/SearchResults'

export default {
  components: {
    SearchResults,
  },
  data() {
    return {
      search: '',
      reviews: [],
    }
  },
  computed: {
    searchedReviews() {
      if (!this.search) return []

      return this.reviews.filter((review) => {
        return review.node.title.toLowerCase().includes(this.search.toLowerCase().trim())
      })
    },
  },
  mounted() {
    this.reviews = this.$static.reviews.edges
  },
}
</script>
