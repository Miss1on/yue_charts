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

const positionMap = [
  { name: "南宁", position: [765.8787271584873, 493.55871563349626] },
];

export default {
  name: "App",

  mounted() {
    axios.get("/svg/china.svg").then((res) => {
      var chartDom = document.getElementById("main");
      var myChart = echarts.init(chartDom);
      echarts.registerMap("iceland_svg", { svg: res.data });
      var option = {
        tooltip: {},
        geo: {
          map: "iceland_svg",
          roam: true,
        },
        series: {
          type: "effectScatter",
          coordinateSystem: "geo",
          geoIndex: 0,
          data: [[...positionMap[0].position, 100]],
        },
      };
      myChart.setOption(option);
      myChart.getZr().on("click", function (params) {
        var pixelPoint = [params.offsetX, params.offsetY];
        var dataPoint = myChart.convertFromPixel({ geoIndex: 0 }, pixelPoint);
        // 在 SVG 上点击时，坐标会被打印。
        // 这些坐标可以在 `series.data` 里使用。
        console.log(dataPoint);
      });
    });
  },
};
</script>
