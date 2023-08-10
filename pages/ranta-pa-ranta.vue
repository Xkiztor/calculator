<script setup lang='ts'>
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
} from 'chart.js'
import { Line } from 'vue-chartjs'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
)

const years = ref(15)
const initial = ref(15000)
const monthlyInvestment = ref(1200)
const annualIncrease = ref(7)

const totalMoney = computed(() => {
  let decimalIncrease = annualIncrease.value / 100

  let months = years.value * 12

  let monthlyReturnRate = 1 + decimalIncrease / 12

  let investmentValue = initial.value

  for (let i = 0; i < months; i++) {
    investmentValue = (investmentValue + monthlyInvestment.value) * monthlyReturnRate
  }

  // return investmentValue.toFixed(2)
  // return investmentValue.toFixed(0)
  if (investmentValue < 10000) {
    return Math.round(investmentValue / 100) * 100
  } else if (investmentValue < 30000) {
    return Math.round(investmentValue / 500) * 500
  } else if (investmentValue < 100000) {
    return Math.round(investmentValue / 1000) * 1000
  } else {
    return Math.round(investmentValue / 10000) * 10000
  }

})

const formatedNumber = computed(() => {
  return totalMoney.value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
})

const moneyArray = computed(() => {

  const annualReturnDecimal = annualIncrease.value / 100;

  const months = years.value * 12;

  const monthlyReturnRate = 1 + annualReturnDecimal / 12;


  const noGrowth = []
  let value = initial.value;
  for (let i = 0; i < months; i++) {
    value = (value + monthlyInvestment.value);
    noGrowth.push(Math.round(value * 100) / 100);
  }

  const investmentValues = [];
  let investmentValue = initial.value;
  for (let i = 0; i < months; i++) {
    investmentValue = (investmentValue + monthlyInvestment.value) * monthlyReturnRate;
    investmentValues.push(Math.round(investmentValue));
    // investmentValues.push(Math.round(investmentValue * 100) / 100);
  }

  const labels = []
  for (let i = 0; i < investmentValues.length; i++) {
    labels.push('Månad: ' + i + '  |  År: ' + Math.ceil(i / 12))
  }

  console.log(investmentValues);

  const data = {
    labels: labels,
    // labels: Array.from({ length: investmentValues.length }, (_, i) => i + 1),
    datasets: [
      {
        label: 'Kr',
        backgroundColor: '#00c281',
        borderColor: "#00c281",
        data: investmentValues,
      },
      {
        label: 'Utan Avkastning',
        backgroundColor: '#2cb3c2',
        borderColor: "#2cb3c2",
        data: noGrowth
      },
    ]
  }

  return data
})

const options = {
  responsive: true,
  interaction: {
    mode: 'index',
    intersect: false,
  },
  scales: {
    x: {
      // display: false,
      title: {
        // display: true,
        color: '#fff',
        text: 'Tid'
      },
      border: {
        // display: false,
        color: '#00616f',
        width: 2,
      },
      grid: {
        display: false
      },
      ticks: {
        display: false
      }
    },
    y: {
      position: 'right',
      // display: false,
      title: {
        // display: true
      },
      border: {
        display: false,

      },
      grid: {
        display: false
      },
      ticks: {
        display: false,
        font: {
          family: "'Roboto', Roboto"
        },
        color: '#fff'
      }
    },
  },
  elements: {
    point: {
      radius: 0
      // pointStyle: 'line'
    },
    // line: {}
  },
  plugins: {
    legend: {
      display: false
    },
    subtitle: {
      display: false
    },
    title: {
      display: false
    },
    tooltip: {
      backgroundColor: 'rgba(7, 56, 62, 1)',
      padding: 12,
      caretPadding: 20,
      caretSize: 0,
      cornerRadius: 12,

      // displayColors: false,
      boxWidth: 14,
      boxHeight: 14,
      boxPadding: 5,
      usePointStyle: true,

      titleFont: {
        weight: 'normal',
        family: "'Roboto', Roboto",
        size: 16,
        lineHeight: 1.5
      },
      bodyFont: {
        weight: 'normal',
        family: "'Roboto', Roboto",
        size: 16,
        lineHeight: 1.5
      }
    },

  }
}
</script>


<template>
  <div class="page ranta-pa-ranta">
    <div class="titles">
      <h1 class="number title">
        <Icon name="ph:chart-line-up-bold" />
        Ränta På Ränta
      </h1>
      <!-- <h2 class="description">Här kan du räkna ut ditt slutliga kapital efter x antal</h2>
      <h2 class="description">år efter investering</h2> -->
      <div class="description">Med denna kan du räkna med ränta-på-rända effekten om du månadssparar på börsen i till
        exempel fonder eller aktier.</div>
      <!-- <h2 class="description"></h2> -->
    </div>

    <div class="card future">
      <div class="padding">
        <div class="big-number">
          <h1 v-if="formatedNumber === 'NaN'" class="number placeholder">- - -</h1>
          <h1 v-else class="number">{{ formatedNumber }} kr</h1>
        </div>
        <div class="inputs">
          <div class="years after">
            <!-- <label>År</label> -->
            <input type="number" v-model="years">
          </div>
          <div class="kronor-start after">
            <!-- <label>Startbelopp</label> -->
            <input type="number" v-model="initial">
          </div>
          <div class="monthly after">
            <!-- <label>Investering per månaden</label> -->
            <input type="number" v-model="monthlyInvestment">
          </div>
          <div class="percentage after">
            <!-- <label>Årlig avkastning</label> -->
            <input type="number" class="percentage" v-model="annualIncrease">
          </div>
        </div>

      </div>
    </div>
    <div class="card graph">
      <div class="padding">
        <Line id="my-chart-id" :data="moneyArray" :options="options"></Line>
      </div>
    </div>
  </div>
</template>


<style>
.page.ranta-pa-ranta {
  max-width: 40rem;
  width: fit-content;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding-bottom: 58px;
}

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
  /* grid-template-columns: 1fr 1fr; */
  grid-template-columns: 1fr;
  place-items: center;
  gap: 1rem;

  width: 100%;

  text-align: center;
}

.future input {
  width: 100%;
}

.card.graph {
  width: 100%;
}

.page.ranta-pa-ranta .card {
  width: 100%;
}

.after {
  position: relative;
}

.after::after {
  position: absolute;
  right: 12px;
  top: 0;
  bottom: 0;
  margin-top: auto;
  height: fit-content;
  margin-bottom: auto;
  /* color: var(--blue-text); */
  /* color: var(--primary); */
  opacity: 0.9;
}

.percentage.after::after {
  content: '% / år';
}

.kronor-start.after::after {
  content: 'kr från start';
}

.years.after::after {
  content: 'år';
}

.monthly.after::after {
  content: 'kr / månaden';
}
</style>