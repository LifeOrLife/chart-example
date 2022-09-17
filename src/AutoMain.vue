<script lang="ts" setup>
// @ts-nocheck
import * as echarts from 'echarts'
import 'echarts-gl';
import { onMounted } from 'vue'
import { getLine } from './area'


let option, geoJson, baseTexture, myChart
onMounted(() => {
    setTimeout(() => {
        let chartDom = document.getElementById('con')!;
        console.log(chartDom)
        myChart = echarts.init(chartDom);
        import('./glob.json').then(res => {
            geoJson = res
            getBaseTexture()
        })
        // geoJson = getLine()
        // getBaseTexture()
    }, 100)

})
// 生成球面纹理
function getBaseTexture() {
    echarts.registerMap('world', geoJson);
    let canvas = document.createElement('canvas');
    baseTexture = echarts.init(canvas, null, {
        width: 4096,
        height: 2048,
    });
    baseTexture.setOption({
        backgroundColor: '#66ccff00',
        geo: {
            type: 'map',
            map: 'world',
            left: 0,
            top: 0,
            right: 0,
            bottom: 0,
            boundingCoords: [
                [-180, 90],
                [180, -90],
            ],
            roam: false,
            itemStyle: {
                normal: {
                    // areaColor: '#2455ad',
                    // borderColor: '#000c2d',
                    areaColor: 'aqua',
                    borderColor: 'aqua',
                },
                emphasis: {
                    // areaColor: '#2455ad',
                    // borderColor: '#000c2d',
                    areaColor: '#20bcb0',
                    borderColor: '#20bcb0',
                }
            },
            label: {
                normal: {
                    fontSize: 20,
                    show: false,
                    textStyle: {
                        color: '#fff',
                    },
                },
                emphasis: {
                    fontSize: 30,
                    show: false,
                    textStyle: {
                        color: 'yellow',
                    },
                },
            },
        },
    });
    drawEarth();
}

// 绘制球体
function drawEarth() {
    option = {
        backgroundColor: 'none',
        globe: {
            baseTexture: baseTexture, // 基础纹理
            globeRadius: 150, // 球面半径
            environment: 'none',
            // environment: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
            // offset: 0, color: '#00aaff' // 天空颜色
            // }, {
            // offset: 0.7, color: '#998866' // 地面颜色
            // }, {
            // offset: 1, color: '#998866' // 地面颜色
            // }], false),
            shading: 'color',
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
            },
            viewControl: {
                alpha: 30,
                beta: 160,
                autoRotate: true, // 开启自动旋转
                autoRotateAfterStill: 10,
                distance: 240,
            },
        },
        color: getColor(30),
        series: [
            {
                name: '世界贸易情况',
                type: 'lines3D',
                coordinateSystem: 'globe',
                effect: {
                    show: true,
                },
                blendMode: 'lighter',
                lineStyle: {
                    width: 2,
                    color: '#66ccff'
                },
                data: [],
                silent: false,
            },
        ],
    };
    // 随机数据 i控制线数量
    for (let i = 0; i < 90; i++) {
        option.series[0].data = option.series[0].data.concat(rodamData());
    }
    myChart.clear();
    myChart.setOption(option, true);
}

function getColor(num) {
    if (!num || num === 1) {
        return '#' + Math.random().toString(16).slice(2)
    }
    const colors = []
    while(num--) {
        colors.push('#' + Math.random().toString().slice(2))
    }
    return colors
}

function rodamData() {
    let name = '随机数据' + Math.random().toFixed(5) * 100000;
    // 模拟数据，构造飞线的起始经纬度
    // let longitude = 116.2;
    // let latitude = 39.56;
    let longitude = Math.random() * 360 - 180;
    let latitude = Math.random() * 180 - 90;
    // let longitude2 = Math.random() * 360 - 180;
    // let latitude2 = Math.random() * 180 - 90;
    const lv = Math.random() * 90
    const ltv =  Math.random() * 18
    let longitude2 = longitude + lv * (Math.random() > 0.5 ? 1 : -1)
    let latitude2 = latitude + ltv * (Math.random() > 0.5 ? 1 : -1)
    return {
        coords: [
            [longitude, latitude],
            [longitude2, latitude2],
        ],
        value: (Math.random() * 3000).toFixed(2),
    };
}
</script>
<template>
    <div class="chart-con" id="con"></div>
</template>
<style scoped>
    .chart-con {
        width: 100vw;
        height: 100vh;
        background-image: linear-gradient(25deg, #2e057e, #464895, #5080ac, #4bbac2);
    }
</style>
