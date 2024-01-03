<template>
    <div id="pmap_container"></div>
    <input v-model="inputValue" />
    <button @click="addMapMarker">Add Marker</button>
</template>

<script>
import AMapLoader from "@amap/amap-jsapi-loader";


window._AMapSecurityConfig = {
    securityJsCode: "49589548a3691130b640aeeee854bca2",
};

export default {
    name: "pmap_container",

    data() {
        return {
            markers: [],
            markersPosition: [],
            inputValue: "", // 用于绑定输入框的值
            editingIndex: -1, // 默认为-1，表示没有点被编辑
        };
    },

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

                // AMap.plugin(
                //     [
                //         "AMap.ToolBar",
                //         "AMap.Scale",
                //         "AMap.HawkEye",
                //         "AMap.MapType",
                //         "AMap.Geolocation",
                //     ],
                //     () => {
                //         //在图面添加工具条控件
                //         this.map.addControl(new AMap.ToolBar());

                //         // 在图面添加比例尺控件
                //         this.map.addControl(new AMap.Scale());

                //         // 在图面添加鹰眼控件
                //         this.map.addControl(new AMap.HawkEye({ isOpen: true }));

                //         // 在图面添加类别切换控件
                //         this.map.addControl(new AMap.MapType());

                //         // 在图面添加定位控件
                //         this.map.addControl(new AMap.Geolocation());
                //     }
                // );
                // this.map.setFitView();


                this.drawMap(); // 调用绘制地图函数

            });
        },
        drawMap() {
            this.map.off("dblclick", this.mapClickFn);
            this.map.on("dblclick", this.mapClickFn);
        },

        mapClickFn(e) {
            if (this.marker) {
                this.marker.setMap(null);
                this.marker = null;
            }
            this.markersPosition = [e.lnglat.lng, e.lnglat.lat];
            this.setMapMarker(e);
        },

        setMapMarker(e) {
            let marker = new AMap.Marker({
                icon: "//a.amap.com/jsapi_demos/static/demo-center/icons/poi-marker-default.png",
                map: this.map,
                position: this.markersPosition,
                offset: new AMap.Pixel(-20, -50),
            });

            this.markers.push(marker);
            this.editingIndex = this.markers.length - 1;

            var lnglat = e.lnglat;
            var geocoder = new AMap.Geocoder({
                city: "全国",
            });

            geocoder.getAddress(lnglat, (status, result) => {
                if (status === "complete" && result.regeocode) {
                    var address = result.regeocode.formattedAddress;
                    this.locatios.push({
                        F_CoordinateX: lnglat.lng,
                        F_CoordinateY: lnglat.lat,
                        F_Position: address,
                        mapId: marker._amap_id,
                        F_CoordinateName: "",
                    });

                    var title = `${address}`,
                        content = [];
                    content.push(`X ：${lnglat.lng}`);
                    content.push(`Y ：${lnglat.lat}`);

                    var infoWindow = new AMap.InfoWindow({
                        isCustom: true,
                        content: this.createInfoWindow(
                            title,
                            content.join("<br/>"),
                            this.locatios,
                            lnglat.lng
                        ),
                        offset: new AMap.Pixel(16, -35),
                    });

                    infoWindow.open(this.map, marker.getPosition());
                    AMap.event.addListener(marker, "click", () => {
                        infoWindow.open(this.map, marker.getPosition());
                    });
                } else {
                    this.$message.error(res.message);
                }
            });
        },

        //自定义信息窗体
        createInfoWindow(title, content, locatios, mapId) {
            let obj = {};
            locatios.map((item) => {
                if (item.F_CoordinateX == mapId) {
                    obj = item;
                    console.log(obj);
                }
            });
            //info 为 信息窗体
            var info = document.createElement("div");
            info.className = "info";

            //可以通过下面的方式修改自定义窗体的宽高
            //info.style.width = "400px";
            // 定义顶部标题
            let top = document.createElement("div");
            let titleD = document.createElement("div");
            let closeX = document.createElement("img");
            top.className = "info-top";
            top.title = obj.name;
            titleD.innerHTML = title;
            closeX.src = "http://webapi.amap.com/images/close2.gif";
            closeX.onclick = this.closeInfoWindow; //点击右上角的x可以关闭该信息窗体

            top.appendChild(titleD);
            top.appendChild(closeX);
            info.appendChild(top); //信息窗体增加顶部的div

            // 定义中部内容
            var middle = document.createElement("div");
            middle.className = "info-middle";
            middle.style.backgroundColor = "white";
            middle.innerHTML = content;
            info.appendChild(middle); //信息窗体增加中部的div

            let user = document.createElement("div");
            user.className = "fillUsers";
            user.innerHTML = "名称 ：";
            let inputs = document.createElement("input");
            inputs.placeholder = "请输入";
            console.log(locatios);
            //edit
            locatios.map((item) => {
                if (item.F_CoordinateX === mapId) {
                    inputs.value = item.F_CoordinateName;
                    console.log(item.F_CoordinateName);
                }
            });
            inputs.oninput = function () {
                _this.inputValue = inputs.value;
                // 定义的是事件被触发后要做的事情
                console.log(locatios);
                locatios.map((item) => {
                    console.log(item.F_CoordinateX);
                    console.log(mapId);
                    if (item.F_CoordinateX === mapId) {
                        item.F_CoordinateName = inputs.value;
                        console.log(item.F_CoordinateName);
                    }
                });
            };
            info.appendChild(user);
            user.appendChild(inputs);

            // 定义底部内容
            var bottom = document.createElement("div");
            bottom.className = "info-bottom";
            bottom.style.position = "relative";
            bottom.style.top = "0px";
            bottom.style.margin = "0 auto";
            var sharp = document.createElement("img");
            sharp.src = "http://webapi.amap.com/images/sharp.png";
            bottom.appendChild(sharp);
            info.appendChild(bottom); //信息窗体增加底部的div

            return info;
        },

        //关闭信息窗体
        closeInfoWindow() {
            this.map.clearInfoWindow();
        },

        addMapMarker() {
            // 你可以在这里添加自定义逻辑，例如在地图上点击按钮时添加标记
            // 调用 addMapMarker 方法，它将处理标记的添加和其他逻辑
            // 你也可以将按钮添加到模板中，通过点击按钮触发这个方法
            // 例如：<button @click="addMapMarker">Add Marker</button>
            // ...
        },
    },
};
</script>

<style scoped>
#pmap_container {
    width: 100%;
    height: 100%;
}

.info {
    border: solid 1px silver;
}

div.info-top {
    font-size: 12px;
    position: relative;
    background: none repeat scroll 0 0 #f9f9f9;
    border-bottom: 1px solid #ccc;
    border-radius: 5px 5px 0 0;
}

div.info-top div {
    display: inline-block;
    color: #333333;
    font-size: 14px;
    font-weight: bold;
    line-height: 32px;
    padding: 0 20px 0 10px;
    width: 300px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

div.info-top img {
    position: absolute;
    top: 10px;
    right: 10px;
    transition-duration: 0.25s;
}

div.info-top img:hover {
    box-shadow: 0px 0px 5px #000;
}

div.info-middle {
    font-size: 13px;
    padding: 7px 7px 0 9px;
    line-height: 24px;
}

.fillUsers {
    background: #fff;
    padding: 0 6px 8px 7px;
    font-size: 13px;
}

div.info-bottom {
    height: 0px;
    width: 100%;
    clear: both;
    text-align: center;
}

span {
    margin-left: 5px;
    font-size: 11px;
}

.info-middle img {
    float: left;
    margin-right: 6px;
}
</style>
