<template>
  <div ref="map" style="width: 800px; height: 500px; border: solid red 1px" />
</template>

<script>
import * as echarts from "echarts";
import data from "./json/data.json";
export default {
  name: "App",

  data() {
    return {
      map: [],
    };
  },

  mounted() {
    echarts.registerMap("map", {
      geoJSON: data,
    });

    var chart = echarts.init(this.$refs.map);
    chart.setOption({
      visualMap: {
        min: 0,
        max: 100,
        text: ["High", "Low"],
        realtime: false,
        calculable: true,
        inRange: {
          color: ["lightskyblue", "yellow", "orangered"],
        },
      },
      title: {
        text: "上海海洋大学建筑热度",
      },
      // geo: [
      //   {
      //     map: "map",
      //   },
      // ],
      series: [
        {
          type: "map",
          map: "map",
          data: [
            {
              name: "上海海洋大学",
              value: 0,
            },
            {
              name: "体育馆",
              value: 70,
            },
            {
              name: "临港校区生活广场",
              value: 20,
            },
            {
              name: "第一餐厅",
              value: 10,
            },
            {
              name: "公共实验室",
              value: 90,
            },
            {
              name: "图书馆",
              value: 50,
            },
          ],
        },
      ],
    });
  },
};
</script>
