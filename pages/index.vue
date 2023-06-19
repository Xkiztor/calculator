<script setup lang='ts'>

</script>


<template>
  <div class="index">
    <div class="text-side">
      <h1 class="page-title">En räknare som den borde vara</h1>
      <p class="description">
        Idén är ganska enkel. Här kan du snabbt räkna ut olika finansiella saker.
      </p>
      <p class="description">
        - Mycket enklare än att använda miniräknare!
      </p>
      <div class="blob text"></div>
    </div>
    <div class="branding">
      <!-- <Icon name="material-symbols:calculate-outline-rounded" /> -->
      <Icon name="lucide:calculator" />
      <div class="blob logo"></div>
    </div>
  </div>
</template>


<style>
.index {
  display: grid;
  grid-template-columns: 1fr 1fr;
  place-items: start center;
  margin-top: 70px;
  padding: 0 150px;
}

.page-title {
  font-family: 'Roboto Slab Bold';
  font-size: 4rem;
}

.description {
  font-size: 1.5rem;
  /* margin: var(--padding) 0; */
}

.branding .icon {
  font-size: 25rem;
}

.branding .icon * {
  color: var(--primary);
}

.index .text-side {
  max-width: 35rem;
}

.branding {
  place-self: start end;
}

.blob {
  background: var(--primary);
  width: 500px;
  height: 500px;
  border-radius: 50%;
  filter: blur(100px);
  z-index: -1;
  position: absolute;
  opacity: 0.15;
}

.blob.text {
  top: 300px;
  left: -100px;
}

.blob.logo {
  top: 100px;
  right: 100px;
}

@media screen and (max-width: 1000px) {
  .index {
    grid-template-columns: 1fr;
    text-align: center;
  }

  .branding {
    display: none;
  }
}

@media screen and (max-width: 1400px) {
  .index {
    padding: 0 50px;
  }
}
</style>