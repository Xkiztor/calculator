<script setup lang='ts'>
const years = ref(5)
const initial = ref(0)
const annualIncrease = ref(7)
const monthlyInvestment = ref(0)

const totalMoney = computed(() => {
  let decimalIncrease = annualIncrease.value / 100

  let months = years.value * 12

  let monthlyReturnRate = 1 + decimalIncrease / 12

  let investmentValue = initial.value

  for (let i = 0; i < months; i++) {
    investmentValue = (investmentValue + monthlyInvestment.value) * monthlyReturnRate
  }

  return investmentValue.toFixed(2)



  let futureInitial = initial.value * Math.pow((1 + decimalIncrease), years.value)

  // let futureMonthlyInvest = (monthlyInvestment.value * 12) * (Math.pow(1 + decimalIncrease, years.value - 1))

  // let futureMonthlyInvest = Math.pow(monthlyInvestment.value * 12 * (decimalIncrease + 1), years.value)

  let futureMonthlyInvested = monthlyInvestment.value * (Math.pow(1 + decimalIncrease, years.value - 1) / decimalIncrease)

  let total = futureInitial + futureMonthlyInvested

  let rounded = Math.round(total * 100) / 100

  return rounded
})

</script>


<template>
  <div class="page framtid">
    <div class="titles">
      <h1 class="number title">
        <Icon name="ph:chart-line-up-bold" />
        Framtidsräknare
      </h1>
      <h2 class="description">Här kan du räkna ut ditt slutliga kapital efter x antal</h2>
      <h2 class="description">år efter investering</h2>
    </div>
    <div class="card future">
      <div class="big-number">
        <h1 class="number">{{ totalMoney }} kr</h1>
      </div>
      <div class="inputs">
        <div>
          <label>År</label>
          <input type="number" v-model="years">
        </div>
        <div>
          <label>Startbelopp</label>
          <input type="number" v-model="initial">
        </div>
        <div>
          <label>Ökning per år</label>
          <input type="number" v-model="annualIncrease">
        </div>
        <div>
          <label>Investering i månaden</label>
          <input type="number" v-model="monthlyInvestment">
        </div>
      </div>
    </div>
  </div>
</template>


<style>
.big-number h1 {
  font-size: 4rem;
  line-height: 1;
  text-align: center;
  margin: 12px 0 32px 0;
}

.future .inputs {
  display: grid;
  grid-template-columns: 1fr;
  gap: 14px;
  place-items: center;
}

.future .inputs div {
  display: grid;
  grid-template-columns: 1fr 1fr;
  place-items: center;
  gap: 1rem;

  text-align: center;
}
</style>