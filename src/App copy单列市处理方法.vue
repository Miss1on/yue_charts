<template>
  <div ref="map" style="width: 800px; height: 500px; border: solid red 1px" />
</template>

<script>
import * as echarts from "echarts";
import chinaJson from "./json/china.json";
import gdsJson from "./json/gds.json";
import xjJson from "./json/xj.json";
export default {
  name: "App",

  data() {
    return {
      map: [],
    };
  },

  methods: {
    // 处理单列市
    dlsHandle(adcode = 440300) {
      // 在单列市json中找到单列市的json配置项
      const dlsJson = xjJson.features.find(
        (i) => i.properties.adcode === adcode
      );
      const newJson = chinaJson;
      // json 文件渲染存在先后顺序，先渲染的在图层底
      newJson.features.push(dlsJson);
      return newJson;
    },
  },
  mounted() {
    echarts.registerMap("map", {
      geoJSON: this.dlsHandle(652800),
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
