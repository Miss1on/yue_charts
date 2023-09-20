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
        // series: {
        //   type: "effectScatter",
        //   coordinateSystem: "geo",
        //   geoIndex: 0,
        //   data: [[...positionMap[0].position, 100]],
        // },
        series: {
          type: "lines",
          coordinateSystem: "geo",
          geoIndex: 0,
          polyline: true,
          data: [
            // coords: [
            //   // 位置是连续的
            //   [829.5182886885827, 341.550107963978], // 起点
            //   [764.8474372267231, 494.06430242091034],
            //   [799.3395295348579, 492.6189715461351],
            //   [832.4922225414307, 474.680875322705],
            // ],

            // 位置非连续
            [
              {
                coord: [829.5182886885827, 341.550107963978],
                lineStyle: {},
              },
              {
                // 终点坐标
                coord: [764.8474372267231, 494.06430242091034],
              },
            ],
            [
              {
                coord: [829.5182886885827, 341.550107963978],
                lineStyle: {},
              },
              {
                // 终点坐标
                coord: [832.4922225414307, 474.680875322705],
              },
            ],
            [
              {
                coord: [829.5182886885827, 341.550107963978],
                lineStyle: {
                  color: "red",
                },
              },
              {
                // 终点坐标
                coord: [711.6786727591304, 430.967112348894],
              },
            ],
            [
              {
                coord: [829.5182886885827, 341.550107963978],
                lineStyle: {
                  color: "red",
                },
              },
              {
                // 终点坐标
                coord: [660.0724786201129, 385.5928437660749],
              },
            ],
          ],
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
