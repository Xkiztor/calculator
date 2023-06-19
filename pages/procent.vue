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
    <div class="titles">
      <h1 class="number title">
        <Icon name="humbleicons:exchange-vertical" />
        Ökning / Minskning
      </h1>
      <h2 class="description">Här kan du räkna ut procentskillnad mellan två tal.</h2>
    </div>
    <div class="card">
      <div class="big-number">
        <h1 v-if="percentageDif && from && to" class="number"
          :class="{ 'positive': isNegative === false, 'negative': isNegative === true }">
          <div class="negative" v-if="isNegative === true && from && to">
            <Icon name="material-symbols:arrow-circle-down" />
          </div>
          <div class="positive" v-if="isNegative === false && from && to">
            <Icon name="material-symbols:arrow-circle-up" />
          </div>
          {{ prefix }}{{ percentageDif }}
          %
        </h1>
        <h1 v-else class="number placeholder">
          <!-- <Icon name="humbleicons:exchange-vertical" /> -->
          <!-- 0% -->
          - - -
        </h1>
      </div>
      <div class="inputs">
        <div>
          <input type="number" placeholder="Från" v-model="from">
        </div>
        <!-- <div class="negative" v-if="isNegative === true && from && to">
          <Icon name="material-symbols:arrow-circle-down" />
        </div>
        <div class="positive" v-else-if="isNegative === false && from && to">
          <Icon name="material-symbols:arrow-circle-up" />
        </div>
        <div v-else class="default">
          <Icon name="material-symbols:arrow-circle-right" />
        </div> -->
        <div>
          <input type="number" placeholder="Till" v-model="to">
        </div>
      </div>
    </div>
  </div>
</template>


<style>
.inputs {
  /* display: grid; */
  /* grid-template-columns: 10fr 2fr 10fr; */
  /* gap: 14px; */
  /* place-items: center; */
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  gap: var(--padding);
}

/* .inputs input {
  width: auto;
} */


.inputs .icon {
  font-size: 2rem;
}

.positive,
.positive * {
  color: var(--positive);
}

.negative,
.negative * {
  color: var(--negative);
}

.default .icon * {
  color: var(--primary);
}

.big-number h1 {
  font-size: 4rem;
  line-height: 1;
  text-align: center;
  margin: 12px 0 32px 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--padding);
}

.big-number .placeholder {
  color: var(--primary);
}

.big-number .icon {
  font-size: 3.5rem;
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


/* .percentage .big-number .icon {
  font-size: 1em;
} */
</style>