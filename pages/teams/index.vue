<template>
  <main>
    <section v-if="posts" class="w-full max-w-5xl mx-auto">
      <h1 class="text-gray-900 mb-12">Dannes Pannband 4: Ta ner Ekborg</h1>
      <posts post-type="teams" :amount="10" />
    </section>
    <section class="w-full max-w-5xl mx-auto">
      <div class="w-full">
        <p v-if="$fetchState.pending">Hämtar laginfo...</p>
        <p v-else-if="$fetchState.error">Ett fel uppstod</p>
        <div v-else>
          <h3 class="mb-4">Spelschema</h3>
          <div class="flex flex-row bg-white shadow-lg" v-for="(meeting, index) in league.results" :key="meeting.id">
            <div v-if="index < 3" class="w-1/2 flex flex-row py-4 px-2 sm:px-4 items-center border-b border-gray-200">
              <div class="team w-1/2 flex flex-col justify-center text-center">
                <p class="font-bold">{{ meeting.entry_1_name }}</p>
                <p class="mt-0 font-bold text-green-600">{{ meeting.entry_1_points }}</p>
              </div>
              <div class="separator py-2 px-4 text-gray-300 mx-0 sm:mx-4 border-r border-l border-gray-200">
                <p class="text-gray-300">GW{{ meeting.event }}</p>
              </div>
              <div class="team w-1/2 flex flex-col justify-center text-center">
                <img />
                <p class="font-bold">{{ meeting.entry_2_name }}</p>
                <p class="mt-0 font-bold text-green-600">{{ meeting.entry_2_points }}</p>
              </div>
            </div>
          </div>
          <br />
          <div class="flex flex-row bg-white shadow-lg" v-for="(meeting, index) in league.results" :key="meeting.id">
            <div
              v-if="index > 3 && index < 7"
              class="w-1/2 flex flex-row py-4 px-2 sm:px-4 items-center border-b border-gray-200"
            >
              <div class="team w-1/2 flex flex-col justify-center text-center">
                <p class="font-bold">{{ meeting.entry_1_name }}</p>
                <p class="mt-0 font-bold text-green-600">{{ meeting.entry_1_points }}</p>
              </div>
              <div class="separator py-2 px-4 text-gray-300 mx-0 sm:mx-4 border-r border-l border-gray-200">
                <p class="text-gray-300">GW{{ meeting.event }}</p>
              </div>
              <div class="team w-1/2 flex flex-col justify-center text-center">
                <img />
                <p class="font-bold">{{ meeting.entry_2_name }}</p>
                <p class="mt-0 font-bold text-green-600">{{ meeting.entry_2_points }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="w-1/3">
        <button id="fetchButton" @click="$fetch" class="btn mt-8">Uppdatera</button>
      </div>
    </section>
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
  data() {
    return {
      league: [],
    }
  },
  async fetch() {
    try {
      this.league = await fetch(
        'https://strawberry-sundae-75499.herokuapp.com/https://fantasy.allsvenskan.se/api/leagues-h2h-matches/league/1414/'
      ).then((res) => res.json())
    } catch (e) {
      error({ message: 'League not found' })
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
