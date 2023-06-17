<script setup lang='ts'>
const mode = ref(1)


const from = ref()
const to = ref()

const isNegative = ref(false)

const percentageDif = computed(() => {
  if (from.value < to.value) {
    //Increase
    isNegative.value = false

    let dif = to.value - from.value
    let percentDif = dif / from.value * 100

    let rounded = Math.round(percentDif * 100) / 100
    return rounded
  }
  else if (from.value > to.value) {
    //Decrease
    isNegative.value = true

    let dif = from.value - to.value
    let percentDif = dif / from.value * 100

    let rounded = Math.round(percentDif * 100) / 100
    return rounded
  }
})

const prefix = computed(() => isNegative.value ? '-' : '')

</script>


<template>
  <div class="page percentage">
    <div class="card">
      <div class="big-number">
        <h1 v-if="percentageDif && from && to" class="number"
          :class="{ 'positive': isNegative === false, 'negative': isNegative === true }">{{ prefix
          }}{{
  percentageDif }}%</h1>
        <h1 v-else class="number">0%</h1>
      </div>
      <div class="inputs">
        <div>
          <input type="number" placeholder="Från" v-model="from">
        </div>
        <div>
          <input type="number" placeholder="Till" v-model="to">
        </div>
      </div>
    </div>
  </div>
</template>


<style>
.inputs {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 14px;
  place-items: center;
}

.inputs input {
  width: auto;
}

.positive {
  color: var(--positive);
}

.negative {
  color: var(--negative);
}

.big-number h1 {
  font-size: 4rem;
  line-height: 1;
  text-align: center;
  margin: 12px 0 32px 0;
}

.page.percentage {
  display: grid;
  gap: var(--padding);
  max-width: 40rem;
}

.percentage .card {
  width: 100%;
}

.percentage .card {
  padding: 22px 14px;
}
</style>