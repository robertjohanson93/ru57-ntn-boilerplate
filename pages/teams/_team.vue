<template>
  <main>
    <section v-if="post">
      <img v-if="post.cover" class="mt-4 mb-4 w-full max-h-96 object-contain" :src="post.cover" />

      <article class="max-w-5xl mx-auto mt-12 mb-12">
        <h1 class="uppercase font-black text-center mb-12">{{ post.title }}</h1>
        <div class="w-full border-2 border-primary rounded-lg flex flex-row justify-center px-4 py-4">
          <div class="flex flex-col justify-center text-center border-r border-grey-500">
            <p>Placering</p>
            <p class="text-4xl text-primary font-black">1</p>
          </div>
          <div class="flex flex-col justify-center text-center border-r border-grey-500">
            <p>Poäng Dannes</p>
            <p class="text-4xl text-primary font-black">27</p>
          </div>
          <div class="flex flex-col justify-center text-center border-r border-grey-500">
            <p>OR</p>
            <p class="text-4xl text-primary font-black">3 000</p>
          </div>
          <div class="flex flex-col justify-center text-center">
            <p>Poäng OR</p>
            <p class="text-4xl text-primary font-black">538</p>
          </div>
        </div>

        <nuxt-content :document="post" />
        <div v-if="post.gallery" class="nuxt-content">
          <img v-for="image in post.gallery" class="image" :key="image.id" :src="image" />
        </div>
      </article>
    </section>
    <section>
      <div>
        <p v-if="$fetchState.pending">Hämtar laginfo...</p>
        <p v-else-if="$fetchState.error">Ett fel uppstod</p>
        <div v-else>
          <h1 class="mb-4">Laginfo</h1>
          <ul class="mb-4">
            <li>Id: {{ user.id }}</li>
            <li>Namn: {{ user.player_first_name }} {{ user.player_last_name }}</li>
          </ul>
          <h1 class="mt-4 mb-4">Klassiska ligor</h1>
          <ul v-for="league in user.leagues.classic" :key="league.id">
            <li>{{ league.name }}</li>
          </ul>
          <h1 class="mt-4 mb-4">Head to head ligor</h1>
          <ul v-for="league in user.leagues.h2h" :key="league.id">
            <li>{{ league.name }}</li>
          </ul>
        </div>
        <button
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
    }
  },
  async fetch() {
    const proxyUrl = 'https://strawberry-sundae-75499.herokuapp.com/'
    let apiUrl = 'https://fantasy.allsvenskan.se/api/entry/1360/'
    let url = proxyUrl + apiUrl
    this.user = await fetch(url).then((res) => res.json())
  },
}
</script>
