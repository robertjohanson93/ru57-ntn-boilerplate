<template>
  <main>
    <section v-if="post">
      <img v-if="post.cover" class="mt-4 mb-4 w-full max-h-96 object-contain" :src="post.cover" />

      <article class="max-w-5xl mx-auto mt-12 mb-12">
        <h1 class="uppercase font-black text-center mb-12 text-gray-900">{{ post.title }}</h1>
        <div class="w-full bg-white rounded-lg flex sm:flex-row flex-col justify-center px-4 py-4">
          <div
            class="history_container flex flex-col justify-center text-center sm:border-r sm:border-b-0 border-b sm:pb-0 sm:pt-0 pb-4 pt-4 border-grey-500"
          >
            <p class="text-gray-900">Placering</p>
            <p class="text-4xl text-primary font-black">-</p>
          </div>
          <div
            class="history_container flex flex-col justify-center text-center sm:border-r sm:border-b-0 border-b sm:pb-0 sm:pt-0 pb-4 pt-4 border-grey-500"
          >
            <p class="text-gray-900">Poäng Dannes</p>
            <p class="text-4xl text-primary font-black">-</p>
          </div>
          <div
            class="history_container flex flex-col justify-center text-center sm:border-r sm:border-b-0 border-b sm:pb-0 sm:pt-0 pb-4 pt-4 border-grey-500"
          >
            <p class="text-gray-900">OR</p>
            <p class="text-4xl text-primary font-black">-</p>
          </div>
          <div
            class="history_container flex flex-col justify-center text-center sm:border-r sm:border-b-0 border-b sm:pb-0 sm:pt-0 pb-4 pt-4 border-grey-500"
          >
            <p class="text-gray-900">Poäng OR</p>
            <p class="text-4xl text-primary font-black">-</p>
          </div>
        </div>

        <nuxt-content :document="post" />
        <div v-if="post.gallery" class="nuxt-content">
          <img v-for="image in post.gallery" class="image" :key="image.id" :src="image" />
        </div>

        <h2 class="mb-4 text-gray-900">Historik</h2>
        <p v-if="$fetchState.pending">Hämtar historik...</p>
        <p v-else-if="$fetchState.error">Ett fel uppstod</p>
        <div v-else class="w-full bg-white rounded-lg flex sm:flex-row flex-col justify-center px-4 py-4">
          <div
            v-for="year in history.past"
            :key="year.id"
            class="history_container flex flex-col justify-center text-center sm:border-r sm:border-b-0 border-b sm:pb-0 sm:pt-0 pb-4 pt-4 border-grey-200"
          >
            <p class="font-bold text-gray-900">{{ year.season_name }}</p>
            <p class="text-gray-900">Poäng</p>
            <p class="text-4xl text-primary font-black">{{ year.total_points }}</p>
            <p class="text-gray-900">Rank</p>
            <p class="text-4xl text-primary font-black">{{ year.rank }}</p>
          </div>
        </div>
      </article>
    </section>
    <section class="hidden">
      <div>
        <p v-if="$fetchState.pending">Hämtar laginfo...</p>
        <p v-else-if="$fetchState.error">Ett fel uppstod</p>
        <div v-else></div>
        <button
          id="fetchButton"
          @click="$fetch"
          class="button bg-transparent hover:bg-primary-900 text-primary-050 font-semibold uppercase py-2 px-4 border border-primary hover:border-primary rounded-full inline-flex items-center mt-8"
        >
          Uppdatera
        </button>
      </div>
    </section>
  </main>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    let post
    try {
      post = await $content('teams', params.team).fetch()
    } catch (e) {
      error({ message: 'Teams not found' })
    }
    return { post }
  },
  data() {
    return {
      user: [],
      history: [],
    }
  },
  async fetch() {
    try {
      this.user = await fetch(
        'https://strawberry-sundae-75499.herokuapp.com/https://fantasy.allsvenskan.se/api/entry/' + this.post.fantasyId
      ).then((res) => res.json())
      this.history = await fetch(
        'https://strawberry-sundae-75499.herokuapp.com/https://fantasy.allsvenskan.se/api/entry/' +
          this.post.fantasyId +
          '/history/'
      ).then((res) => res.json())
    } catch (e) {
      error({ message: 'Data not found' })
    }
  },
}

if (process.browser) {
  window.onNuxtReady((app) => {
    const button = document.getElementById('fetchButton')
    button.click()
  })
}
</script>
