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
    axios.get("/svg/data.svg").then((res) => {
      echarts.registerMap("map", {
        svg: res.data,
      });

      var chart = echarts.init(this.$refs.map);
      chart.setOption({
        geo: [
          {
            map: "map",
          },
        ],
      });
    });
  },
};
</script>
