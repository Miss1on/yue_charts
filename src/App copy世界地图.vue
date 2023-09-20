<template>
  <div ref="map" style="width: 800px; height: 500px; border: solid red 1px" />
</template>

<script>
import * as echarts from "echarts";
import world from "./json/000000_EW.json";
export default {
  name: "App",

  data() {
    return {
      map: [],
    };
  },

  mounted() {
    echarts.registerMap("map", {
      geoJSON: world,
    });

    var chart = echarts.init(this.$refs.map);
    chart.setOption({
      geo: [
        {
          map: "map",
        },
      ],
    });
  },
};
</script>
