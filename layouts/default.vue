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
    <nav>
      <nuxt-link to="/" class="logo">
        <h1>
          AvanzaRäknaren
          <Icon name="material-symbols:calculate" />
        </h1>
      </nuxt-link>
      <nuxt-link to="procent">Ökning / Minskning</nuxt-link>
      <nuxt-link to="framtid">Framtidsräknare</nuxt-link>
      <nuxt-link to="dca">Dollar Cost Average</nuxt-link>
    </nav>
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
  /* padding: var(--padding); */
  display: grid;
  place-items: start center;
}

nav {
  height: 70px;
  border-bottom: 1px solid var(--border-color);
  margin-bottom: calc(var(--padding) * 3);
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: var(--padding);
}

nav a {
  padding: var(--padding) var(--padding) 6px;
  border-bottom: 2px solid rgba(0, 0, 0, 0);
  text-decoration: none;
}

nav a.router-link-active:not(.logo) {
  border-color: var(--primary);
}

nav h1 {
  margin: 0 calc(var(--padding) * 3) 0 var(--padding);
  font-size: 1.2rem;
}

nav h1 .icon {
  font-size: 2rem;
}

nav h1 .icon * {
  color: var(--primary);
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type=number] {
  -moz-appearance: textfield;
}
</style>