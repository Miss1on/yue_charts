<template>
  <div ref="map" style="width: 800px; height: 500px; border: solid red 1px" />
</template>

<script>
import * as echarts from "echarts";
import chinaJson from "./json/china.json";
export default {
  name: "App",
  mounted() {
    console.log(chinaJson);
    echarts.registerMap("china", { geoJSON: chinaJson });
    var chart = echarts.init(this.$refs.map);
    chart.setOption({
      geo: [
        {
          map: "china",
        },
      ],
    });
  },
};
</script>
