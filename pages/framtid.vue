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

interface yearType {
  year: number,
  monthlyInvest: number,
  changed: boolean,
}

const years = ref(5)
const yearlyIncrease = ref(7)

const yearsArray: Ref<yearType[]> = ref([])



const updateYearsArray = () => {
  yearsArray.value = []
  for (let i = 0; i < years.value; i++) {
    const year = {
      year: i + 1,
      monthlyInvest: 100,
      changed: false,
    }

    yearsArray.value.push(year)
  }
}
updateYearsArray()

watch(years, () => {
  console.log('years updated');
  updateYearsArray()
})


const changeYear = (whatYear: number, newInvestValue: string) => {
  console.log(whatYear);
  console.log(newInvestValue);

  let formattedInvest = parseFloat(newInvestValue)

  yearsArray.value[whatYear - 1].monthlyInvest = formattedInvest

  yearsArray.value[whatYear - 1].changed = true

  for (let i = 0; i < yearsArray.value.length; i++) {
    if (i > whatYear - 1) {
      if (yearsArray.value[i].changed === true) {
        console.log('changed');
        return
      } else {
        console.log('not changed');
        yearsArray.value[i].monthlyInvest = formattedInvest
      }
    }
  }
}


const monthlyInvestments = computed(() => {
  const array = []
  for (let i = 0; i < yearsArray.value.length; i++) {
    array.push(yearsArray.value[i].monthlyInvest)
  }
  return array
})


const totalMoney = computed(() => {
  const totalMonths = years.value * 12
  let totalAmount = 0

  for (let i = 0; i < totalMonths; i++) {
    const yearIndex = Math.floor(i / 12); // Determine the current year based on the number of months passed
    const monthlyInvestment = monthlyInvestments.value[yearIndex] || 0; // Get the monthly investment for the current year
    totalAmount += monthlyInvestment; // Add the monthly investment to the total amount

    totalAmount += totalAmount * (yearlyIncrease.value / 100) / 12; // Apply the annual increase at the end of each month

  }

  return totalAmount
})

const totalMoneyArray = computed(() => {
  const totalMonths = years.value * 12;
  const totalAmounts = [];
  let totalAmount = 0;

  for (let i = 0; i < totalMonths; i++) {
    const yearIndex = Math.floor(i / 12); // Determine the current year based on the number of months passed
    const monthlyInvestment = monthlyInvestments.value[yearIndex] || 0; // Get the monthly investment for the current year
    totalAmount += monthlyInvestment; // Add the monthly investment to the total amount

    totalAmount += totalAmount * (yearlyIncrease.value / 100) / 12; // Apply the annual increase at the end of each month

    totalAmounts.push(totalAmount); // Add the current total amount to the array
  }

  return totalAmounts;
})

const graphData = computed(() => {
  const labels = []

  for (let i = 0; i < totalMoneyArray.value.length; i++) {
    labels.push(`Månad: ${i}  |  År: ${Math.ceil(i / 12)}`)
  }

  const data = {
    labels: labels,
    // labels: Array.from({ length: investmentValues.length }, (_, i) => i + 1),
    datasets: [
      {
        label: 'Kr',
        backgroundColor: '#00c281',
        borderColor: "#00c281",
        data: totalMoneyArray.value,
      },
      // {
      //   label: 'Månadssparande',
      //   backgroundColor: '#2cb3c2',
      //   borderColor: "#2cb3c2",
      //   data: monthlyInvestments.value
      // },
    ]
  }

  console.log(data);

  return data
})

const formattedTotalMoney = computed(() => {
  let roundedMoney

  if (totalMoney.value < 10000) {
    roundedMoney = Math.round(totalMoney.value / 100) * 100
  } else if (totalMoney.value < 30000) {
    roundedMoney = Math.round(totalMoney.value / 500) * 500
  } else if (totalMoney.value < 100000) {
    roundedMoney = Math.round(totalMoney.value / 1000) * 1000
  } else {
    roundedMoney = Math.round(totalMoney.value / 10000) * 10000
  }

  return roundedMoney.toFixed().toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")
})
</script>


<template>
  <div class="page framtid">
    <div class="titles">
      <h1 class="number title">
        <Icon name="ic:round-timeline" />
        Framtidsräknaren
      </h1>
      <!-- <h2 class="description">Här kan du räkna ut ditt slutliga kapital efter x antal</h2>
      <h2 class="description">år efter investering</h2> -->
      <h2 class="description">I denna räknare kan du ändra ditt månadssparande år till år.</h2>
      <!-- <h2 class="description"></h2> -->
    </div>
    <div class="card">
      <div>
        <h1 class="number big-number">{{ formattedTotalMoney }} kr</h1>
      </div>
      <div class="settings">
        <div class="after years">
          <!-- <label for="">Antal år</label> -->
          <input type="number" v-model.number="years">
        </div>
        <div class="after annual-increase">
          <!-- <label for="">Årlig Ökning %</label> -->
          <input type="number" v-model.number="yearlyIncrease">
        </div>
      </div>
    </div>
    <div>
      <div class="years card">
        <div v-for="year in yearsArray" class="year">
          <label>År {{ year.year }}</label>
          <!-- <h3>{{ year.monthlyInvest }} </h3> -->
          <div class="after kronor">
            <input type="number" :value="year.monthlyInvest" @input.number="changeYear(year.year, $event?.target?.value)">
          </div>
          <!-- <input type="number" v-model="year.monthlyInvest" @change="changeYear(year.year, year.monthlyInvest)"> -->
        </div>
      </div>
    </div>
    <div class="card graph">
      <div class="padding">
        <Line id="my-chart-id" :data="graphData" :options="options"></Line>
      </div>
    </div>
  </div>
</template>


<style>
.page.framtid {
  max-width: 40rem;
  width: fit-content;
  display: flex;
  flex-direction: column;
  /* align-items: flex-start; */
  padding-bottom: 58px;
}

.page.framtid .card {
  padding: var(--padding);
  width: 100%;
}

.year {
  display: flex;
  padding-left: var(--padding);
  align-items: center;
  gap: var(--padding);
  margin: var(--padding) 0;
}

.year .after,
.year .after input {
  display: flex;
  flex-grow: 1;
}

.page.framtid .settings {
  display: flex;
  flex-direction: column;
  gap: var(--padding);
}

.page.framtid .settings,
.page.framtid .settings input {
  width: 100%;
}

.framtid .big-number {
  font-size: 4rem;
  line-height: 1;
  text-align: center;
  margin: 12px 0 32px 0;
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

.kronor.after::after {
  content: 'kr / månaden';
}

.years.after::after {
  content: 'år';
}

.annual-increase.after::after {
  content: '% / år';
}
</style>