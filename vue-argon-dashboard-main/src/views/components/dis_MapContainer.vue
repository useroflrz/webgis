
<template>
    <div id="dis_MapContainer"></div>
</template>

<script>
import AMapLoader from "@amap/amap-jsapi-loader";

var MAP2 = null;

window._AMapSecurityConfig = {
    securityJsCode: "49589548a3691130b640aeeee854bca2",
};

export default {
    name: "dis_MapContainer",
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
                plugins: ['Map3D', 'AMap.DistrictSearch'], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
                Loca: {
                    version: "2.0.0"
                }
            })
                .then((AMap) => {

                    this.map = new AMap.Map('dis_MapContainer', {
                        zooms: [3, 20],
                        zoom: 4.8,
                        center: [109, 32],
                        showLabel: false,
                        mapStyle: 'amap://styles/dark',
                        pitch: 50,
                        viewMode: '3D',
                    });

                    for (var i = 0; i < capitals.length; i += 1) {
                        var center = capitals[i].center;
                        var circleMarker = new AMap.CircleMarker({
                            center: center,
                            radius: 10 + Math.random() * 10,//3D视图下，CircleMarker半径不要超过64px
                            strokeColor: 'white',
                            strokeWeight: 2,
                            strokeOpacity: 0.5,
                            fillColor: 'rgba(0,0,255,1)',
                            fillOpacity: 0.5,
                            zIndex: 10,
                            bubble: true,
                            cursor: 'pointer',
                            clickable: true
                        })
                        circleMarker.setMap(this.map)
                    }

                    this.loca = new Loca.Container({
                        map: this.map,
                    });

                    var geo = new Loca.GeoJSONSource({
                        url: 'https://a.amap.com/Loca/static/loca-v2/demos/mock_data/tsing.json',
                    });

                    var heatmap = new Loca.HeatMapLayer({
                        // loca,
                        zIndex: 10,
                        opacity: 1,
                        visible: true,
                        zooms: [2, 22],
                    });

                    heatmap.setSource(geo, {
                        radius: 9000,
                        unit: 'meter',
                        height: 8000,
                        difference: true,
                        gradient: {
                            1: '#FF4C2F',
                            0.8: '#FAA53F',
                            0.6: '#FFF100',
                            0.5: '#7DF675',
                            0.4: '#5CE182',
                            0.2: '#29CF6F',
                        },
                        value: function (index, feature) {
                            return feature.properties.count;
                        },
                        opacity: [0, 1],
                        heightBezier: [0, 0.53, 0.37, 0.98]
                    });

                    this.loca.add(heatmap);

                    MAP2 = this.map;

                    MAP2.on('click', () => {
                        heatmap.addAnimate({
                            key: 'radius',
                            value: [0, 1],
                            random: true,
                            transform: 1000,
                            delay: 6000,
                            easing: 'BounceOut' //https://redmed.github.io/chito/example/easing.html
                        });
                    });



                })
        },


    },
};
</script>



<style scoped>
#dis_MapContainer {
    width: 100%;
    height: 100%;
}
</style>