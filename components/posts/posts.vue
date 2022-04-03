<template>
  <div v-if="posts.length > 0" class="my-8">
    <div class="flex flex-wrap sm:-mx-2">
      <div v-for="(post, index) in posts" :key="index" class="sm:w-1/3 w-full px-2 py-2">
        <nuxt-link
          :to="`${post.slug}`"
          class="card card--clickable rounded overflow-hidden shadow-lg bg-white text-center border border-transparent hover:border-2 hover:border-green-600"
        >
          <template v-if="postType === 'teams'">
            <img v-if="post.cover" class="w-1/2 px-4 py-4 mx-auto" :src="post.cover" />
            <div class="flex flex-col px-4 py-4">
              <h3 class="font-bold text-xl text-gray-900 mb-2">{{ post.title }}</h3>
            </div>
          </template>

          <template v-else>
            <span class="w-full">
              <span class="flex justify-between align-baseline px-4 py-4">
                <h3 class="font-bold text-gray-900 text-xl mb-2">{{ post.title }}</h3>
              </span>
              <p class="mt-2 px-4 pb-4 text-gray-700">{{ post.description }}</p>
            </span>
          </template>
        </nuxt-link>
      </div>
    </div>
  </div>
  <div v-else-if="loading" class="cards">
    <div v-for="placeholder in placeholderClasses" :key="placeholder.id" class="card">
      <content-placeholders :rounded="true" :class="placeholder">
        <content-placeholders-heading />
      </content-placeholders>
    </div>
  </div>
  <p v-else class="max-w-5xl mx-auto">
    {{ amount > 1 ? 'Posts not found' : 'Post not found' }}
  </p>
</template>

<script>
export default {
  name: 'Posts',
  props: {
    postType: {
      type: String,
      default: 'blog',
      validator: (val) => ['blog', 'teams'].includes(val),
    },
    amount: {
      // ? https://content.nuxtjs.org/fetching#limitn
      type: Number,
      default: 10,
      validator: (val) => val >= 0 && val < 100,
    },
    sortBy: {
      // ? https://content.nuxtjs.org/fetching#sortbykey-direction
      type: Object,
      default: () => ({
        key: 'slug',
        direction: 'desc', // you probably want 'asc' here
      }),
      validator: (obj) => typeof obj.key === 'string' && typeof obj.direction === 'string',
    },
  },
  data() {
    return {
      posts: [],
      loading: true,
    }
  },
  computed: {
    placeholderClasses() {
      const classes = ['w-full', 'w-2/3', 'w-5/6']
      return [...Array.from({ length: this.amount }, (v, i) => classes[i % classes.length])] // repeats classes after one another
    },
  },
  async mounted() {
    this.loading = true
    this.posts = await this.fetchPosts()
    this.loading = false
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString(process.env.lang) || ''
    },
    async fetchPosts(postType = this.postType, amount = this.amount, sortBy = this.sortBy) {
      return this.$content(postType)
        .sortBy(sortBy.key, sortBy.direction)
        .limit(amount)
        .fetch()
        .catch((err) => console.error(err) || [])
    },
  },
}
</script>
