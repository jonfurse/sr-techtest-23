<script setup>
  import {GoogleCharts} from 'google-charts';
  import {defineProps, computed, ref} from "vue";

  const props = defineProps( {
    chartData : ref([])
  })

  GoogleCharts.load(drawChart);

  function drawChart() {

    let rowData = [];
    props.chartData.forEach((item) => {
      rowData.push([item.name, item.count]);
    })
    console.log(rowData[0].length)
    var data = new google.visualization.DataTable();
    data.addColumn('string', 'Sectors');
    data.addColumn('number', 'Count');
    data.addRows(
      rowData

    );

    // Set chart options
    var options = {'title':'Count of interactions by sector',
      'width':1000,
      'height':800};
      const pie_1_chart = new GoogleCharts.api.visualization.PieChart(document.getElementById('chart1'));
      pie_1_chart.draw(data, options)
    // }
  }
</script>

<template>
  <div>
    <div id="chart1"></div>
  </div>
</template>

