<script setup>
  import { ref, computed, onMounted } from 'vue';
  import axios from "axios";
  import Chart from "./Chart.vue";

  const data = ref({
    interactions: [],
  })

  const fetchInteractions = async () => {
    const result = await axios.get('http://substantiveresearch.pythonanywhere.com/')
    data.value.interactions = result.data;
  }
  onMounted(fetchInteractions)

  const interactionSectors = computed(() => {
    return data.value.interactions.reduce( (bySector, interaction) => {
      let foundSector = bySector.find(sector => sector['name'] === interaction['name'])
      if (foundSector) {
        foundSector.count++
        foundSector.dates.push(interaction['date'])
      }
      else {
        bySector.push({
          'name': interaction['name'],
          'count': 1,
          'dates': [interaction['date']]
        })
      }
      return bySector
    }, [])
  })

  const interactionsCount = computed( () => {
    return data.value.interactions.length
  })

</script>

<template>
  <div class="container">
    <span>Interactions by Sector</span>
    <span>count = {{interactionsCount}}</span>
<!--    <ul>-->
<!--      <li v-for="item in interactionSectors" :key="item">-->
<!--        <span>Name: {{item.name  }}   count: {{item.count}} </span>-->
<!--      </li>-->
<!--    </ul>-->
    <Chart :chart-data="interactionSectors"/>
  </div>
</template>