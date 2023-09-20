<template>
  <div ref="map" style="width: 800px; height: 500px; border: solid red 1px" />
</template>

<script>
import * as echarts from "echarts";
import chinaJson from "./json/anqin.json";
export default {
  name: "App",

  data() {
    return {
      map: [],
    };
  },

  methods: {
    // 生成映射文件
    setDataMap() {
      const data = require("./json/china.json");
      this.map = [];
      data.features.forEach((i) => {
        this.map.push({
          name: i.properties.name,
          adcode: i.properties.adcode,
        });
      });
    },

    // 拿到省份对应的 json 文件
    getGeoJson(list) {
      const adcodeList = [];
      const json = {
        type: "FeatureCollection",
        features: [],
      };
      const data = require("./json/china.json");

      // 先获取 adcode
      list.forEach((item) => {
        adcodeList.push(this.map.find((i) => i.name === item)?.adcode);
      });
      adcodeList.forEach((code) => {
        json.features.push(
          data.features.find((i) => i.properties.adcode === code)
        );
      });
      // 返回拼凑的json文件
      return json;
    },
  },
  mounted() {
    this.setDataMap();
    // 宁夏    新疆   青海   陕西   甘肃
    const qhJson = this.getGeoJson(["宁夏回族自治区", "新疆维吾尔自治区", "青海省", "陕西省", "甘肃省"]);

    echarts.registerMap("mapName", {
      geoJSON: qhJson,
    });

    var chart = echarts.init(this.$refs.map);
    chart.setOption({
      geo: [
        {
          map: "mapName",
        },
      ],
    });
  },
};
</script>
