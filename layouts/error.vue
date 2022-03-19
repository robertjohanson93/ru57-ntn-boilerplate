<template>
  <main>
    <section>
      <template v-if="error.statusCode === 404">
        <h1 class="title">{{ error.message || 'Sidan hittades inte' }}</h1>
        <h2 class="subtitle">404</h2>
        <p class="mb-4">Det verkar som att du följde en felaktig länk eller skrev en URL som inte existerar.</p>
      </template>
      <template v-else>
        <h1 class="title">Ett fel uppstod</h1>
        <p class="mb-4">Det verkar som att du följde en felaktig länk eller skrev en URL som inte existerar.</p>
      </template>
      <nav class="mt-12" aria-label="go to home">
        <router-back to="/" class="block" />
      </nav>
    </section>
  </main>
</template>

<script>
export default {
  name: 'error',
  props: ['error'],
}
</script>
