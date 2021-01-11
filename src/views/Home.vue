<template>
  <div id="mapBox" class="map_box"></div>
</template>

<script>
import { defineComponent, reactive, onMounted} from 'vue';

export default defineComponent({
  name: 'Home',
  components:{
    
  },
  setup(props,ctx){
    const state = reactive({
      myChart: echarts.Echarts
    })
    let map = reactive({});
    const initMap = (dom, data, zoom) => {
        state.myChart = echarts.init(dom);
        let option = {
            animation: false,
            bmap: { // 加载 bmap 组件
                center: [104.114129, 37.550339],
                zoom: zoom, // 地图当前的缩放比例
                roam: true, // 开启鼠标缩放和平移漫游
                // enableMapClick: false
                // opt: {
                //     enableMapClick: false,
                // },
            },
            series: {
                name: '测试',
                type: 'effectScatter', // 带有涟漪特效动画的散点（气泡）图
                coordinateSystem: 'bmap',
                symbol: state.icon, // 使用自定义的SVG图标
                symbolSize: [10, 10],
                legendHoverLink: false,
                z: 6,
                effectType: 'ripple',
                rippleEffect: {
                    period: 2, // 涟漪特效的动画周期
                    scale: 5, // 涟漪特效动画中波纹的最大缩放比例
                    brushType: 'fill' // 涟漪特效的波纹绘制方式
                },
                itemStyle: {
                    normal: {
                    color: 'red'
                    },
                    emphasis: {
                    color: 'red'
                    }
                },
                label: {
                    show: true,
                    position: 'inside',
                    formatter:(v) => {
                        var text= v.data.count;
                        return text;
                    },
                },
                data: data
            }
        };
        state.myChart.setOption(option);
        map = state.myChart.getModel().getComponent('bmap').getBMap();
    };
    onMounted(() => {
      console.log('mounted!');
      state.dom = document.getElementById('mapBox');
      initMap(state.dom , [], 4);
      addImgMarker();
    });
    const addImgMarker = () => {
        const point = new BMap.Point(-0.51, 9.24)
        const myIcon = new BMap.Icon('https://lbsyun.baidu.com/jsdemo/img/fox.gif', new BMap.Size(300, 157));
        const marker = new BMap.Marker(point, {
            icon: myIcon
        });
        map.addOverlay(marker);
    };
  }
});
</script>

<style scoped>
.map_box {
    width: 100%;
    height: 100%;
}
</style>
