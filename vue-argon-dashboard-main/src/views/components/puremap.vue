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
                key: "b8f1d1072c8346e4a93a5e901a6811fb",
                version: "2.0",
                plugins: [],
            }).then((AMap) => {
                this.map = new AMap.Map("pmap_container", {
                    zoom: 10,
                    center: [116.397428, 39.90923],
                    mapStyle: "amap://styles/whitesmoke",
                    viewMode: "2D",
                });

                AMap.plugin(
                    [
                        "AMap.ToolBar",
                        "AMap.Scale",
                        "AMap.HawkEye",
                        "AMap.MapType",
                        "AMap.Geolocation",
                    ],
                    () => {
                        // 在图面添加工具条控件
                        //this.map.addControl(new AMap.ToolBar());

                        // 在图面添加比例尺控件
                        this.map.addControl(new AMap.Scale());

                        // 在图面添加鹰眼控件
                        this.map.addControl(new AMap.HawkEye({ isOpen: true }));

                        // 在图面添加类别切换控件
                        //this.map.addControl(new AMap.MapType());

                        // 在图面添加定位控件
                        //this.map.addControl(new AMap.Geolocation());
                    }
                );

                var lnglats = [
                    { "lng": 116.368904, "lat": 39.923423 },
                    { "lng": 116.382122, "lat": 39.921176 },
                    { "lng": 116.387271, "lat": 39.922501 },
                    { "lng": 116.398258, "lat": 39.914600 },
                    { "lng": 113.936718, "lat": 22.535183 },
                    { "lng": 113.391792, "lat": 23.062033 },
                    { "lng": 113.058683, "lat": 23.132339 },
                    { "lng": 91.160914, "lat": 29.63438 },
                    { "lng": 87.579371, "lat": 43.88505 },
                    { "lng": 126.514918, "lat": 45.847 },
                    { "lng": 122.526881, "lat": 52.98753 }
                ];

                for (var i = 0; i < lnglats.length; i++) {
                    var marker = new AMap.Marker({
                        position: [lnglats[i].lng, lnglats[i].lat],
                        map: this.map
                    });
                    marker.content = "我是第" + (i + 1) + "个Marker";
                    marker.on("click", this.markerClick);
                    marker.emit("click", { target: marker });
                }
                this.map.setFitView();
            });
        },
        markerClick(e) {
            var infoWindow = new AMap.InfoWindow({ offset: new AMap.Pixel(0, -30) });
            infoWindow.setContent(e.target.content);
            infoWindow.open(this.map, e.target.getPosition());
        }
    }
};
</script>

<style scoped>
#pmap_container {
    width: 100%;
    height: 100%;
}
</style>
