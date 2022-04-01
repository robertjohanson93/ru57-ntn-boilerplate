<template>
  <main>
    <section v-if="posts" class="w-full max-w-5xl mx-auto">
      <h1 class="title mb-12">Dannes Pannband 4: Ta ner Ekborg</h1>
      <!--
      <div class="hidden">
        <p v-if="$fetchState.pending">Hämtar laginfo...</p>
        <p v-else-if="$fetchState.error">Ett fel uppstod</p>
        <div v-else v-for="team in league.new_entries.results" :key="team.id">
          <p>{{ team.player_first_name }} {{ team.player_last_name }}</p>
        </div>
      </div>
      -->
      <posts post-type="teams" :amount="10" />
    </section>
    <!--
    <button
      id="fetchButton"
      @click="$fetch"
      class="hidden button bg-transparent hover:bg-primary-900 text-primary-050 font-semibold uppercase py-2 px-4 border border-primary hover:border-primary rounded-full inline-flex items-center mt-8"
    >
      Uppdatera
    </button>
    -->
  </main>
</template>

<script>
export default {
  async asyncData({ $content, error }) {
    let posts
    try {
      posts = await $content('teams').fetch()
    } catch (e) {
      error({ message: 'Teams not found' })
    }
    return { posts }
  },
  /**
   * data() {
    return {
      league: [],
    }
  },
  async fetch() {
    try {
      this.league = await fetch(
        'https://strawberry-sundae-75499.herokuapp.com/https://fantasy.allsvenskan.se/api/leagues-h2h/1414/standings'
      ).then((res) => res.json())
    } catch (e) {
      error({ message: 'League not found' })
    }
  },
   */
}

/**
 *
 * if (process.browser) {
 * window.onNuxtReady((app) => {
 *  const button = document.getElementById('fetchButton')
 *    button.click()
 * })
 * }
 */
</script>
