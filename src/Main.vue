<script lang="ts" setup>
import * as echarts from 'echarts';
import 'echarts-gl';
import { points, china } from './data'

let ds = [{
        name: '中国',
        point: [116.46, 39.92, 0],
        itemStyleColor: '#f00',
        labelText: '中国•3000'
    }, {
        name: '印度',
        point: [78.96288, 20.593684, 0],
        itemStyleColor: '#99CC66',
        labelText: '印度•500'
    }, {
        name: '意大利',
        point: [12.56738, 41.87194, 0],
        itemStyleColor: '#9999FF',
        labelText: '意大利•200'
    }, {
        name: '新西兰',
        point: [174.885971, -40.900557, 0],
        itemStyleColor: '#339966',
        labelText: '新西兰•10'
    }, {
        name: '英国',
        point: [-3.435973, 55.378051, 0],
        itemStyleColor: '#993366',
        labelText: '英国•1000'
    }, {
        name: '德国',
        point: [10.451526, 51.165691, 0],
        itemStyleColor: '#996666',
        labelText: '德国•200'
    }, {
        name: '美国',
        point: [-95.712891, 37.09024, 0],
        itemStyleColor: '#66CCFF',
        labelText: '美国•2200'
    }, {
        name: '日本',
        point: [138.252924, 36.204824, 0],
        itemStyleColor: '#666666',
        labelText: '日本•2500'
    }]

    // 点配置信息
    let series = ds.map(item => {
        return {
            name: item.name, // 是否显示左上角图例
            type: 'scatter3D',
            coordinateSystem: 'globe',
            blendMode: 'lighter',
            symbolSize: 16, // 点位大小

            itemStyle: {
                color: item.itemStyleColor, // 各个点位的颜色设置
                opacity: 1, // 透明度
                borderWidth: 1, // 边框宽度
                borderColor: 'rgba(255,255,255,0.8)' //rgba(180, 31, 107, 0.8)
            },
            data: [item.point] // 数据来源
        }
    })
var ROOT_PATH = 'https://echarts.apache.org/examples';

var chartDom = document.getElementById('app')!;
var myChart = echarts.init(chartDom);
var option;

option = {
  backgroundColor: '#000',
  globe: {
    baseTexture: 'src/bg4.jpg',
    displacementScale: 0.1,
    shading: 'lambert',
    light: {
      // 光照阴影
      main: {
        color: '#fff', // 光照颜色
        intensity: 1, // 光照强度
        alpha: 40,
        beta: -30,
      },
      ambient: {
        color: '#fff',
        intensity: 1,
      },
    }
  },
  series: {
    type: 'lines3D',
      coordinateSystem: 'globe',
      blendMode: 'lighter',
      effect: {
          show: true,
          trailWidth: 2,
          trailLength: 0.2
        },
      lineStyle: {
        width: 2,
        color: '#20bcb0',
        opacity: 0.5
      },
      data: china
  }
};

option && myChart.setOption(option);
// https://www.makeapie.cn/echarts
</script>
<template>
    <div class="container"></div>
</template>
