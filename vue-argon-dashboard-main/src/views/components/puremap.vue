<template>
    <div id="pmap_container"></div>
</template>

<script>
import AMapLoader from "@amap/amap-jsapi-loader";


window._AMapSecurityConfig = {
    securityJsCode: "49589548a3691130b640aeeee854bca2",
};

export default {
    name: "pmap_container",
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
            })
                .then((AMap) => {

                    this.map = new AMap.Map("pmap_container", {
                        
                        zoom: 4, // 初始化地图级别
                        center: [116.397428, 39.90923], // 初始化地图中心点位置
                    });
                   
                })

        },


    },
};
</script>

<style scoped>
#pmap_container {
    width: 100%;
    height: 100%;
}
</style>
