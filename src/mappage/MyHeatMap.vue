<template>
  <div class="hello">
    <h3>Ecosystem</h3>
    <highcharts :options="chartOptions"></highcharts>
    <myechart :options="polar"/>
    <myechart :options="loadHeatMap"/>

  </div>
</template>

<script>
import {Chart} from 'highcharts-vue'
import ECharts from 'vue-echarts'
import 'echarts/lib/chart/line'
import 'echarts/lib/component/polar'
import 'echarts/lib/chart/map'
import 'echarts/lib/component/geo'
import 'echarts/lib/chart/scatter'
import 'echarts/lib/chart/effectScatter'



var polarData = [];
for (let i = 0; i <= 360; i++) {
        let t = i / 180 * Math.PI
        let r = Math.sin(2 * t) * Math.cos(2 * t)
        polarData.push([r, i])
}
var myData = function(){
    var res = [];
    res.push({
        name: "其他",
        value: [118.03,25.58,30]
    });
    res.push({
        name: "广州B",
        value: [120.03,21.58,25]
    });
    res.push({
        name: "惠州",
        value: [120.03,21.58,85]
    });
    res.push({
        name: "汕头",
        value: [110.03,22.58,50]
    });
    res.push({
        name: "广州A",
        value: [113.23,23.16,185]
    });
    res.push({
        name: "深圳",
        value: [114.07,22.62,115]
    });
    res.push({
        name: "武汉",
        value: [114.31,30.52,115]
    });
    return res;
}
function buildHeatMap(){
  return {
      title: {
          text: '全国主要城市空气质量 - 百度地图2',
          subtext: 'data from PM25.in',
          sublink: 'http://www.pm25.in',
          left: 'center'
      },
      tooltip : {
          trigger: 'item'
      },
      bmap: {
          center: [113.114129, 23.550339],
          zoom: 5,
          roam: true,
          mapStyle: {
              styleJson: [{
                  'featureType': 'water',
                  'elementType': 'all',
                  'stylers': {
                      'color': '#d1d1d1'
                  }
              }, {
                  'featureType': 'land',
                  'elementType': 'all',
                  'stylers': {
                      'color': '#f3f3f3'
                  }
              }, {
                  'featureType': 'railway',
                  'elementType': 'all',
                  'stylers': {
                      'visibility': 'off'
                  }
              }, {
                  'featureType': 'highway',
                  'elementType': 'all',
                  'stylers': {
                      'color': '#fdfdfd'
                  }
              }, {
                  'featureType': 'highway',
                  'elementType': 'labels',
                  'stylers': {
                      'visibility': 'off'
                  }
              }, {
                  'featureType': 'arterial',
                  'elementType': 'geometry',
                  'stylers': {
                      'color': '#fefefe'
                  }
              }, {
                  'featureType': 'arterial',
                  'elementType': 'geometry.fill',
                  'stylers': {
                      'color': '#fefefe'
                  }
              }, {
                  'featureType': 'poi',
                  'elementType': 'all',
                  'stylers': {
                      'visibility': 'off'
                  }
              }, {
                  'featureType': 'green',
                  'elementType': 'all',
                  'stylers': {
                      'visibility': 'off'
                  }
              }, {
                  'featureType': 'subway',
                  'elementType': 'all',
                  'stylers': {
                      'visibility': 'off'
                  }
              }, {
                  'featureType': 'manmade',
                  'elementType': 'all',
                  'stylers': {
                      'color': '#d1d1d1'
                  }
              }, {
                  'featureType': 'local',
                  'elementType': 'all',
                  'stylers': {
                      'color': '#d1d1d1'
                  }
              }, {
                  'featureType': 'arterial',
                  'elementType': 'labels',
                  'stylers': {
                      'visibility': 'off'
                  }
              }, {
                  'featureType': 'boundary',
                  'elementType': 'all',
                  'stylers': {
                      'color': '#fefefe'
                  }
              }, {
                  'featureType': 'building',
                  'elementType': 'all',
                  'stylers': {
                      'color': '#d1d1d1'
                  }
              }, {
                  'featureType': 'label',
                  'elementType': 'labels.text.fill',
                  'stylers': {
                      'color': '#999999'
                  }
              }]
          }
      },
      series : [
          {
              name: 'pm2.5',
              type: 'scatter',
              coordinateSystem: 'geo',
              data: myData(),
              symbolSize: function (val) {
                  return val[2] / 10;
              },
              encode: {
                  value: 2
              },
              label: {
                  formatter: '{b}',
                  position: 'right',
                  show: false
              },
              itemStyle: {
                  color: 'purple'
              },
              emphasis: {
                  label: {
                      show: true
                  }
              }
          },
          {
              name: 'Top 2',
              type: 'effectScatter',
              coordinateSystem: 'bmap',
              data: myData().sort(function (a, b) {
                  return b.value[2] - a.value[2];
              }).slice(0, 2),
              symbolSize: function (val) {
                  return val[2] / 10;
              },
              encode: {
                  value: 2
              },
              showEffectOn: 'render',
              rippleEffect: {
                  brushType: 'stroke'
              },
              hoverAnimation: true,
              label: {
                  formatter: '{b}',
                  position: 'right',
                  show: true
              },
              itemStyle: {
                  color: 'purple',
                  shadowBlur: 10,
                  shadowColor: '#333'
              },
              zlevel: 1
          }
      ]
  }
}
function buildPolar(){
  return {
      title: {
          text: '极坐标双数值轴'
      },
      legend: {
          data: ['line']
      },
      polar: {
          center: ['50%', '54%']
      },
      tooltip: {
          trigger: 'axis',
          axisPointer: {
              type: 'cross'
          }
      },
      angleAxis: {
          type: 'value',
          startAngle: 0
      },
      radiusAxis: {
          min: 0
      },
      series: [
          {
            coordinateSystem: 'polar',
            name: 'line',
            type: 'line',
            showSymbol: false,
            data: polarData
            }
       ],
       animationDuration: 2000
   };
}
export default {
  name: 'HelloWorld',
  components: {
    highcharts: Chart,
    myechart: ECharts,
  },
  data() {
    return {
      chartOptions: {
          title: {
            text: '折线图'
          },        series: [{
          data: [1,3,2,3] // sample data
        }]
      },
      polar:buildPolar(),
      loadHeatMap: buildHeatMap()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
