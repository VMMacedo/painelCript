<template>
  <div>
    <canvas :id="idChart"></canvas>
  </div>
</template>

<script>


export default {
  name: "Chart",


  props: ["idChart", "labels", "backgroundColor", "title", "dataCoin"],
  data() {
    return {};
  },
  computed: {},
  methods: {},
  mounted() {
    const labels = this.labels;
    const dataCoin = this.dataCoin;

    const data = {
      labels: labels,
      datasets: [
        {
          label: this.title,
          backgroundColor: this.backgroundColor,
          borderColor: this.backgroundColor,
          data: dataCoin,
        },
      ],
    };

    const config = {
      type: "line",
      data: data,
      options: {},
    };
    const myChart = new Chart(document.getElementById(this.idChart), config);
  },
};
</script>

<style>
</style>