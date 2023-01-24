<script setup>
  import { ref, computed, onMounted } from 'vue';
  import axios from "axios";

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
    <ul>
      <li v-for="item in interactionSectors" :key="item">
        <span>Name: {{item.name  }}   count: {{item.count}} </span>
      </li>
    </ul>
  </div>
</template>





<!-- COUNT WORKS BUT EVERY ELEMENT INSIDE OWN ARRAY! -->
<!--const interactionSectors = computed(() => {-->
<!--let bySector = [];-->
<!--let i  = data.value.interactions.reduce(function (bySector, interaction) {-->
<!--let sectorArray = bySector[interaction['sector_id']] =-->
<!--bySector[interaction['sector_id']] ?-->
<!--(bySector[interaction['sector_id']]) :-->
<!--[{-->
<!--'name': interaction['name'],-->
<!--'count': 0,-->
<!--'dates': []-->
<!--}]-->

<!--sectorArray[0].dates.push( interaction['date'] )-->
<!--sectorArray[0].count++-->
<!--//console.log('x', sectorArray)-->

<!--return bySector-->
<!--}, [])-->
<!--console.log('i', JSON.stringify(i))-->
<!--return i-->
<!--})-->