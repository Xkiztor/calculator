<script setup lang='ts'>
import 'assets/main.css'

useHead({
  title: 'Avanza Räknaren',
  meta: [{
    name: 'description',
    content: 'Ett smidig hjälpmedel för att räkna ut olika finansiella saker'
  }],
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
          Avanza Räknaren
          <Icon name="material-symbols:calculate" />
        </h1>
      </nuxt-link>
      <nuxt-link to="procent">Ökning / Minskning</nuxt-link>
      <nuxt-link to="ranta-pa-ranta">Ränta på Ränta</nuxt-link>
      <nuxt-link to="framtid">Framtidsräknare</nuxt-link>
    </nav>
    <slot />
  </div>
</template>


<style>
.card {
  background: var(--element-color);
  box-shadow: var(--box-shadow);
  border-radius: var(--border-radius);
  padding: var(--padding);
  width: fit-content;
  margin-bottom: 21px;
}

.page-container {
  /* min-width: 100%; */
  /* min-height: 100%; */
  /* padding: var(--padding); */
  /* display: grid; */
  /* place-items: start center; */
  display: flex;
  flex-direction: column;
  align-items: center;
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
  appearance: textfield;
  -moz-appearance: textfield;
}
</style>