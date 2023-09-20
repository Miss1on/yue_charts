<template>
  <div class="box">
    <div ref="map" style="width: 800px; height: 500px; border: solid red 1px" />
    <span @click="toback">返回</span>
  </div>
</template>

<script>
import * as echarts from "echarts";
import axios from "axios";
export default {
  name: "App",

  data() {
    return {
      map: [],
      links: [],
      myChart: null,
    };
  },

  watch: {
    links: {
      handler(val) {
        if (!val.length) return;
        this.initParamsCity(val[val.length - 1]); // 始终渲染最后一个数据
      },
      immediate: true,
      deep: true,
    },
  },

  methods: {
    async initParamsCity(adcode) {
      // 如果传了adcode ，则渲染指定 adcode的省份，不然渲染中国地图
      const { data } = await axios.get(`/alljson/${adcode || "100000"}.json`);

      echarts.registerMap("mapJson", { geoJSON: data });

      // 销毁上一次地图实例
      if (this.myChart) this.myChart.dispose();

      this.myChart = echarts.init(this.$refs.map);

      this.myChart.setOption({ geo: [{ map: "mapJson" }] });

      this.myChart.on("click", (params) => {
        // 通过注册的json获取adcode
        this.links.push(this.getAdocode(data, params.name));
      });
    },

    // 查找 adcode
    getAdocode(list, name) {
      return list.features.find((i) => i.properties.name === name).properties
        .adcode;
    },

    toback() {
      if (this.links.length === 1) return; // 最后的中国地图不能再删除了
      this.links.pop();
    },
  },
  mounted() {
    this.links.push("10000");
  },
};
</script>

<style scoped>
.box {
  width: 800px;
  height: 500px;
  position: relative;
}
span {
  top: 20px;
  padding: 5px;
  cursor: pointer;
  position: absolute;
  right: 20px;
  display: inline-block;
  z-index: 999;
  background-color: #6be6c1;
}
</style>
