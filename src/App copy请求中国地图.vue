<template>
  <div ref="map" style="width: 800px; height: 500px"></div>
</template>

<script>
import * as echarts from "echarts";
import axios from "axios";

export default {
  name: "App",

  mounted() {
    axios
      .get("https://geo.datav.aliyun.com/areas_v3/bound/100000_full.json")
      .then(({ data: res }) => {
        echarts.registerMap("china", { geoJSON: res });
        var chart = echarts.init(this.$refs.map);
        chart.setOption({
          geo: [
            {
              map: "china",
            },
          ],
        });
      });
  },
};
</script>
