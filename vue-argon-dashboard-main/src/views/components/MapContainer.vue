

<template>
    <div id="container"></div>
</template>

<script>
import AMapLoader from "@amap/amap-jsapi-loader";


var MAP1 = null;
window._AMapSecurityConfig = {
    securityJsCode: "49589548a3691130b640aeeee854bca2",
};

export default {
    name: "MapContainer",
    mounted() {
        this.initAMap();
    },
    unmounted() {
        this.map?.destroy();
    },
    methods: {
        initAMap() {
            AMapLoader.load({
                key: "b8f1d1072c8346e4a93a5e901a6811fb", // 申请好的Web端开发者Key，首次调用 load 时必填
                version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
                plugins: [], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
                Loca: {
                    version: "2.0.0"
                }
            })
                .then((AMap) => {

                    this.map = new AMap.Map("container", {
                        // 设置地图容器id
                        viewMode: "3D", // 是否为3D地图模式
                        zoom: 4, // 初始化地图级别
                        center: [116.397428, 39.90923], // 初始化地图中心点位置
                        pitch: 40,
                        mapStyle: 'amap://styles/dark',
                        showLabel: false,

                    });
                    this.loca = new Loca.Container({
                        map: this.map
                    });

                    var geo = new Loca.GeoJSONSource({
                        url: 'https://a.amap.com/Loca/static/loca-v2/demos/mock_data/traffic.json',
                    });

                    var heatmap = new Loca.HeatMapLayer({
                        // loca,
                        zIndex: 10,
                        opacity: 1,
                        visible: true,
                        zooms: [2, 22],
                    });

                    heatmap.setSource(geo, {
                        radius: 200000,
                        unit: 'meter',
                        height: 500000,
                        //radius: 35,
                        //unit: 'px',
                        //height: 100,
                        gradient: {
                            0.1: '#2A85B8',
                            0.2: '#16B0A9',
                            0.3: '#29CF6F',
                            0.4: '#5CE182',
                            0.5: '#7DF675',
                            0.6: '#FFF100',
                            0.7: '#FAA53F',
                            1: '#D04343',
                        },
                        value: function (index, feature) {
                            return feature.properties.avg;
                            // var value = feature.properties.mom.slice(0, -1);
                            // return value + 10 * Math.random();
                        },
                        // min: -100,
                        // max: 100,
                        heightBezier: [0, .53, .37, .98],
                    });


                    this.loca.add(heatmap);

                    this.map.on('complete', function () {
                        heatmap.addAnimate({
                            key: 'height',
                            value: [0, 1],
                            duration: 2000,
                            easing: 'BackOut',
                            // yoyo: true,
                            // repeat: 2,
                        });
                        heatmap.addAnimate({
                            key: 'radius',
                            value: [0, 1],
                            duration: 2000,
                            easing: 'BackOut',
                            // 开启随机动画
                            transform: 1000,
                            random: true,
                            delay: 3000,
                        });
                    });


                    MAP1 = this.map;

                    MAP1.on('click', function (e) {
                        var feat = heatmap.queryFeature(e.pixel.toArray());
                        if (feat) {
                            MAP1.clearMap();
                            MAP1.add(new AMap.Marker({
                                position: feat.lnglat,
                                anchor: 'bottom-center',
                                content:
                                    '<div style="margin-bottom: 15px; border:1px solid #fff; border-radius: 4px;color: #fff; width: 150px; text-align: center;">爱心值: ' + feat.value.toFixed(2) + '</div>'

                            }));
                        }

                    });

                })


        },


    },
};
</script>

<style scoped>
#container {
    width: 100%;
    height: 100%;
}
</style>




<!-- <script setup>


let map = null;

onMounted(() => {
    AMapLoader.load({
        key: "b8f1d1072c8346e4a93a5e901a6811fb", // 申请好的Web端开发者Key，首次调用 load 时必填
        version: "3.2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        plugins: [], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
    })
        .then((AMap) => {
            map = new AMap.Map("container", {
                // 设置地图容器id
                viewMode: "3D", // 是否为3D地图模式
                zoom: 11, // 初始化地图级别
                center: [116.397428, 39.90923], // 初始化地图中心点位置
            });
        })
        .catch((e) => {
            console.log(e);
        });
});

onUnmounted(() => {
    map?.destroy();
});
</script> -->



<!-- <style scoped>
#container {
    width: 100%;
    height: 800px;
}
</style> -->
