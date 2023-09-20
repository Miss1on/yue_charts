<template>
  <div class="box">
    <div ref="map" style="width: 800px; height: 500px; border: solid red 1px" />
    <span @click="toback">返回</span>
  </div>
</template>

<script>
import * as echarts from "echarts";
import chinaJson from "./alljson/china.json";
import guangxi from "./alljson/guangxi.json";
import liuzhou from "./alljson/liuzhou.json";
import liucheng from "./alljson/liucheng.json";
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
    initParamsCity(name) {
      // 有返回记录
      // 要有城市映射
      const map = {
        中国: chinaJson,
        广西壮族自治区: guangxi,
        柳州市: liuzhou,
        柳城县: liucheng,
      };
      echarts.registerMap("mapJson", {
        geoJSON: map[name],
      });

      // 销毁上一次地图实例
      if (this.myChart) this.myChart.dispose();
      this.myChart = echarts.init(this.$refs.map);

      this.myChart.setOption({
        geo: [
          {
            map: "mapJson",
          },
        ],
      });
      this.myChart.on("click", (params) => {
        this.links.push(params.name);
      });
    },

    toback() {
      if (this.links.length === 1) return; // 最后的中国地图不能再删除了
      this.links.pop();
    },
  },
  mounted() {
    this.links.push("中国");
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
