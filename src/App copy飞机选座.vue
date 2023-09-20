<template>
  <div
    id="main"
    ref="map"
    style="width: 800px; height: 500px; border: solid red 1px"
  />
</template>

<script>
import * as echarts from "echarts";
import axios from "axios";

export default {
  name: "App",

  mounted() {
    axios.get("/svg/fly.svg").then((res) => {
      var chartDom = document.getElementById("main");
      var myChart = echarts.init(chartDom);
      echarts.registerMap("iceland_svg", { svg: res.data });
      var option = {
        tooltip: {},
        geo: {
          map: "iceland_svg",
          roam: true,
          selectedMode: "multiple",
          select: {
            itemStyle: {
              color: "green",
            },
          },
        },
      };
      myChart.setOption(option);
    });
  },
};
</script>
