<template>
  <nav class="bg-white">
    <div class="max-w-5xl mx-auto">
      <div class="flex items-center justify-between py-4">
        <div class="flex items-center justify-between">
          <div class="flex-shrink-0">
            <a href="/" class="flex flex-row items-center nav__logo"
              ><img class="h-16 w-16 mr-4" src="../../static/img/toast.png" alt="RU57 logo" /><span
                class="text-gray-900 font-black text-4xl"
                >RU57</span
              ></a
            >
          </div>
          <div class="hidden md:block">
            <div class="ml-10 flex items-baseline">
              <a
                href="/teams"
                class="ml-4 px-3 py-2 text-gray-900 font-semibold uppercase py-2 px-4 border-b-4 border-transparent hover:border-green-600"
                >Lagen</a
              >
              <a
                href="/blog"
                class="ml-4 px-3 py-2 text-gray-900 font-semibold uppercase py-2 px-4 border-b-4 border-transparent hover:border-green-600"
                >Nyheter</a
              >
            </div>
          </div>
        </div>
        <div class="-mr-2 flex md:hidden justify-end">
          <!-- Mobile menu button -->
          <button
            @click="toggle"
            class="inline-flex items-center justify-center p-2 rounded-full text-green-600 hover:text-green-500 focus:outline-none focus:text-green-500"
          >
            <svg
              :class="[isOpen ? 'hidden' : 'block', 'h-6 w-6']"
              stroke="currentColor"
              fill="none"
              viewBox="0 0 24 24"
            >
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
            </svg>
            <svg
              :class="[isOpen ? 'block' : 'hidden', 'h-6 w-6']"
              stroke="currentColor"
              fill="none"
              viewBox="0 0 24 24"
            >
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
            </svg>
          </button>
        </div>
      </div>
    </div>
    <div :class="[isOpen ? '' : 'hidden', 'md:hidden']">
      <div class="px-2 pt-2 pb-3 sm:px-3 flex flex-col">
        <a
          href="/teams"
          class="px-3 py-2 text-gray-900 font-semibold uppercase py-2 px-4 border-b-4 border-transparent hover:border-green-600"
          >Lagen</a
        >
        <a
          href="/blog"
          class="px-3 py-2 text-gray-900 font-semibold uppercase py-2 px-4 border-b-4 border-transparent hover:border-green-600"
          >Nyheter</a
        >
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'Header',
  data() {
    return {
      isOpen: false,
    }
  },
  methods: {
    toggle() {
      this.isOpen = !this.isOpen
    },
  },
}
</script>

<style lang="postcss" scoped>
.scrim-bg {
  &::before {
    content: '';
    z-index: -1;
    background-color: var(--bg);
    @apply absolute bottom-0 inset-x-0 h-12 mb-4 transition-colors duration-200 ease-in-out;
  }
  &::after {
    content: '';
    z-index: -1;
    opacity: 1;
    animation: fadeIn1 500ms ease-in-out;
    @apply pointer-events-none absolute bottom-0 inset-x-0 h-16 -mb-12;
    background: linear-gradient(to bottom, #111827, cubic-bezier(0.15, 0, 0.45, 1), transparent);
  }
}
.nuxt-link-exact-active {
  @apply text-gray-200 border-gray-400 bg-gray-800 bg-opacity-25 cursor-default;
}

.light {
  & .scrim-bg {
    &::after {
      animation-name: fadeIn2;
      background: linear-gradient(to bottom, #e5e7eb, cubic-bezier(0.15, 0, 0.45, 1), transparent);
    }
  }
  & .nuxt-link-exact-active {
    @apply text-primary-700 border-gray-600 bg-gray-100;
  }
}

.show {
  display: inline-block;
}

/* Need two because of smoother switching between color modes */
@keyframes fadeIn1 {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@keyframes fadeIn2 {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
