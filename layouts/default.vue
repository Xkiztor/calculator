<script setup lang='ts'>
import 'assets/main.css'

useHead({
  title: 'Calculator',
  htmlAttrs: {
    lang: 'se',
  },
  link: [
    {
      rel: 'preconnect',
      href: 'https://fonts.googleapis.com',
    },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=Roboto&display=swap',
      crossorigin: '',
    },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=Roboto+Slab&display=swap',
      crossorigin: '',
    },
  ],
});
</script>


<template>
  <div class="page-container">
    <slot />
  </div>
</template>


<style>
html {
  background: var(--bg-color);
  font-family: 'Roboto', Roboto, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  min-height: 100vh;
  min-width: 100vw;
}

* {
  color: var(--text-color);
  font-size: 1.1rem;
}

input {
  background: var(--inline-highlight);
  border: 1px solid var(--border-highlight);
  padding: var(--padding);
  border-radius: var(--small-border-radius);
}

body {
  margin: 0;
}

.number {
  font-family: 'Roboto Slab', 'Roboto', Roboto, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-weight: 900;
}

.card {
  background: var(--element-color);
  box-shadow: var(--box-shadow);
  border-radius: var(--border-radius);
  padding: var(--padding);
  width: fit-content;
}

.page-container {
  /* min-width: 100%; */
  /* min-height: 100%; */
  padding: var(--padding);
  display: grid;
  place-items: start center;
}
</style>