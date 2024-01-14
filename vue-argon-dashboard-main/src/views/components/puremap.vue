<template>
    <div id="CONT">
        <div id="pmap_container"></div>
        <div class="kj">
            <div class="dropdown">
                <button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton"
                    data-bs-toggle="dropdown" aria-expanded="false">
                    地图控件
                </button>
                <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton">
                    <li><a class="dropdown-item" href="#" @click="showDiv('div1')">添加活动点</a></li>
                    <li><a class="dropdown-item" href="#" @click="showDiv('div2')">删除活动点</a></li>
                    <li><a class="dropdown-item" href="#" @click="showDiv('')">隐藏控件</a></li>
                </ul>
            </div>



            <div v-if="selectedOption === 'div2'">
                <h3>选项2的内容</h3>
                <!-- 添加选项2的具体内容 -->
            </div>
        </div>
        <div v-if="selectedOption === 'div1'" class="custom-component">
            <div class="container">
                <div class="row justify-content-center">
                    <div class="col-lg-12">
                        <div class="card my-4">
                            <div class="card-body">
                                <h5 class="card-title text-center text-muted">添加活动点</h5>
                                <div class="input-group mb-3">
                                    <div class="input-group-prepend">
                                        <span class="input-group-text">地 址</span>
                                    </div>
                                    <input v-model="address" type="text" class="form-control" @keydown="handleKeyDown">
                                </div>
                                <div class="input-group mb-3">
                                    <div class="input-group-prepend">
                                        <span class="input-group-text">经纬度</span>
                                    </div>
                                    <input id="lnglat" disabled type="text" class="form-control">
                                </div>

                                <div class="text-center">
                                    <button id="geo" type="button" class="btn btn-primary" @click="geoCode">添加</button>
                                </div>

                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import AMapLoader from "@amap/amap-jsapi-loader";

window._AMapSecurityConfig = {
    securityJsCode: "49589548a3691130b640aeeee854bca2",
};

// var that = this;
export default {
    name: "pmap_container",

    data() {
        return {
            map: null,
            markers: [],
            selectedOption: '',
            address: null,
            // markersPosition: [],
            // inputValue: "", // 用于绑定输入框的值
            // editingIndex: -1, // 默认为-1，表示没有点被编辑
        };
    },

    mounted() {
        this.initAMap();
    },
    unmounted() {
        this.map?.destroy();
    },

    methods: {
        showDiv(option) {
            this.selectedOption = option;
        },
        initAMap() {
            AMapLoader.load({
                key: "b8f1d1072c8346e4a93a5e901a6811fb",
                version: "2.0",
                plugins: ["AMap.Geocoder"],
            })
                .then((AMap) => {
                    this.map = new AMap.Map("pmap_container", {
                        zoom: 4,
                    });
                });
        },

        // setMapMarker(e) {


        //     var title;
        //     var content = [];

        //     geocoder.getLocation(address, (status, result) => {
        //         if (status === "complete" && result.geocodes.length) {
        //             var lnglat = result.geocodes[0].location;
        //             document.getElementById('lnglat').value = lnglat;
        //             marker.setPosition(lnglat);
        //             map.add(marker);
        //             map.setFitView(marker);

        //             title = `${address}`,
        //                 content = [];   //内容
        //             // infoWindow.open(this.map, marker.getPosition());
        //         } else {
        //             log.error('根据地址查询位置失败');
        //         }
        //     });



        //     var infoWindow = new AMap.InfoWindow({
        //         isCustom: true,
        //         content:
        //             this.createInfoWindow(
        //                 title,
        //                 content.join("<br/>"),
        //             ),
        //         // offset: new AMap.Pixel(16, -35),
        //     });

        //     var marker = new AMap.Marker({
        //         icon: "//a.amap.com/jsapi_demos/static/demo-center/icons/poi-marker-default.png",
        //         map: this.map,
        //         position: this.markersPosition,
        //         offset: new AMap.Pixel(-20, -50),
        //     });

        //     this.markers.push(marker);
        //     this.editingIndex = this.markers.length - 1;



        //     marker.on(marker, "click", () => {
        //         infoWindow.open(this.map, marker.getPosition());
        //     });

        // },


        geoCode() {
            var geocoder = new AMap.Geocoder();
            var marker = new AMap.Marker({
                icon: "//a.amap.com/jsapi_demos/static/demo-center/icons/poi-marker-default.png",
                map: this.map,
                position: this.markersPosition,
                offset: new AMap.Pixel(-20, -50),
            });

            // infowidnow 的 innerHTML
            var infoWindowContent = `
                <div class="container" style="max-height: 300px; overflow-y: auto;">
                    <div class="card">
                    <div class="card-body">
                        <form>
                        <div class="mb-3">
                            <label for="activityName" class="form-label">活动名</label>
                            <input type="text" class="form-control" id="activityName" placeholder="请输入活动名">
                        </div>
                        <div class="mb-3">
                            <label for="activityCount" class="form-label">活动人数</label>
                            <input type="number" class="form-control" id="activityCount" placeholder="请输入活动人数">
                        </div>
                        <div class="mb-3">
                            <label for="activityTime" class="form-label">活动时间</label>
                            <input type="text" class="form-control" id="activityTime" placeholder="请输入活动时间">
                        </div>
                        </form>
                    </div>
                    </div>
                </div>
                `;

            // 在这里可以使用 infoWindowContent 变量，例如将其添加到页面中的某个元素

            var infoWindow = new AMap.InfoWindow({
                isCustom: true,  //使用自定义窗体
                content: infoWindowContent,
                offset: new AMap.Pixel(16, -45)
            });

            var address = this.address;
            // var addresses = document.getElementById('address').textContent.trim().split('\n');
            var that = this;
            geocoder.getLocation(address, function (status, result) {
                if (status === 'complete' && result.geocodes.length) {

                    // for (var i = 0; i < result.geocodes.length; i += 1) {
                    //     var marker = new AMap.Marker({
                    //         position: result.geocodes[i].location
                    //     });

                    that.markers.push(marker);
                    // }
                    var lnglat = result.geocodes[0].location
                    document.getElementById('lnglat').value = lnglat;
                    marker.setPosition(lnglat);

                    // marker.content = '我是第' + (1) + '个Marker';

                    marker.on('click', (e) => {
                        // infoWindow.setContent(e.target.content);/
                        infoWindow.open(that.map, e.target.getPosition());
                    });

                    that.map.on('click',()=>{
                        that.map.clearInfoWindow();
                    })

                    marker.emit('click', { target: marker });
                } else {
                    log.error('根据地址查询位置失败');
                }
            });
        },

        handleKeyDown(e) {
            if (e.keyCode === 13) {
                geoCode();
                return false;
            }
            return true;
        },


        // //自定义信息窗体
        // createInfoWindow(title, content) {
        //     let obj = {};
        //     //info 为 信息窗体
        //     var info = document.createElement("div");
        //     info.className = "info";

        //     //可以通过下面的方式修改自定义窗体的宽高
        //     //info.style.width = "400px";
        //     // 定义顶部标题
        //     let top = document.createElement("div");
        //     let titleD = document.createElement("div");
        //     let closeX = document.createElement("img");
        //     top.className = "info-top";
        //     top.title = obj.name;
        //     titleD.innerHTML = title;
        //     closeX.src = "http://webapi.amap.com/images/close2.gif";
        //     closeX.onclick = this.closeInfoWindow; //点击右上角的x可以关闭该信息窗体

        //     top.appendChild(titleD);
        //     top.appendChild(closeX);
        //     info.appendChild(top); //信息窗体增加顶部的div

        //     // 定义中部内容
        //     var middle = document.createElement("div");
        //     middle.className = "info-middle";
        //     middle.style.backgroundColor = "white";
        //     middle.innerHTML = content;
        //     info.appendChild(middle); //信息窗体增加中部的div

        //     let user = document.createElement("div");
        //     user.className = "fillUsers";
        //     user.innerHTML = "名称 ：";
        //     let inputs = document.createElement("input");
        //     inputs.placeholder = "请输入";
        //     //edit
        //     inputs.oninput = function () {
        //         this.inputValue = inputs.value;
        //         // 定义的是事件被触发后要做的事情
        //     };
        //     info.appendChild(user);
        //     user.appendChild(inputs);

        //     // 定义底部内容
        //     var bottom = document.createElement("div");
        //     bottom.className = "info-bottom";
        //     bottom.style.position = "relative";
        //     bottom.style.top = "0px";
        //     bottom.style.margin = "0 auto";
        //     var sharp = document.createElement("img");
        //     sharp.src = "http://webapi.amap.com/images/sharp.png";
        //     bottom.appendChild(sharp);
        //     info.appendChild(bottom); //信息窗体增加底部的div

        //     return info;
        // },

        // //关闭信息窗体
        // closeInfoWindow() {
        //     this.map.clearInfoWindow();
        // },

    },
};
</script>

<style scoped>
#pmap_container {
    position: absolute;
    width: 100%;
    height: 100%;
    z-index: 1;
}

#CONT {
    position: relative;
    width: 1150px;
    height: 600px;
}

.btn {
    width: 10rem;
}

.kj {
    position: absolute;

    z-index: 9;
}

.custom-component {
    position: absolute;
    width: 350px;
    height: 300px;
    bottom: 0;
    right: 0;
    z-index: 9;
}
</style>