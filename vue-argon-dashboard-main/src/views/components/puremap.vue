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
        <div v-if="selectedOption === 'div2'" id="tishi">
            <div class="text-center">
                <button id="geo" type="button" class="btn btn-primary" @click="delemarker">点我再点击地图上的活动点</button>
            </div>
        </div>
    </div>
</template>

<script>
import AMapLoader from "@amap/amap-jsapi-loader";

export default {
    name: "pmap_container",

    data() {
        return {
            map: null,
            makerList: [],
            geodata: [],
            selectedOption: '',
            address: null,
            markersPosition: [],
            infoWindowContent: null,

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
                securityJsCode: "49589548a3691130b640aeeee854bca2",
                version: "2.0",
                plugins: ["AMap.Geocoder"],
            })
                .then((AMap) => {
                    this.map = new AMap.Map("pmap_container", {
                        zoom: 4,
                    });

                    this.infoWindowContent = `
                <div class="container" style="max-height: 300px; overflow-y: auto;">
                    <div class="card">
                    <div class="card-body">
                        <form>
                        <div class="mb-3">
                            <label for="activityName" class="form-label">活动名</label>
                            <input type="text" class="form-control" id="activityName" placeholder="请输入活动名" value="${markerInfo.event_name}">
                        </div>
                        <div class="mb-3">
                            <label for="activityCount" class="form-label">活动人数</label>
                            <input type="number" class="form-control" id="activityCount" placeholder="请输入活动人数" value="${markerInfo.event_attendees}">
                        </div>
                        <div class="mb-3">
                            <label for="activityTime" class="form-label">活动时间</label>
                            <input type="text" class="form-control" id="activityTime" placeholder="请输入活动时间" value="${markerInfo.event_time}">
                        </div>
                        </form>
                    </div>
                    </div>
                </div>
                `;
                });

            this.readJSONFile()
        },

        readJSONFile() {
            const url = 'http://127.0.0.1:5500/src/assets/data1.json';
            $.ajax({
                method: "GET",
                url: url,
                dataType: "json",

                success: (data) => {
                    // 在成功读取 JSON 文件后，可以在这里处理数据
                    console.log(data);

                    // 可以通过 data.geodata 访问坐标数组
                    for (let i = 0; i < data.length; i++) {
                        this.geodata.push(data[i]);
                    }
                    console.log(this.geodata);

                    this.setMapMarker()
                },
                error: function (xhr, status, error) {

                    // 处理读取 JSON 文件失败的情况
                    console.log("读取 JSON 文件失败：" + error);
                }

            });
        },

        setMapMarker() {
            for (let i = 0; i < this.geodata.length; i++) {
                // 创建Marker
                var marker = new AMap.Marker({
                    map: this.map,
                    zIndex: 9,
                    offset: new AMap.Pixel(-13, -30),
                    position: this.geodata[i].coordinates
                });

                // 创建InfoWindow
                var infoWindow = new AMap.InfoWindow({
                    isCustom: true,
                    content: this.infoWindowContent, // 初始内容为模板字符串
                    offset: new AMap.Pixel(16, -45)
                });

                // Marker点击事件
                marker.on('click', (e) => {
                    // 获取当前Marker的信息
                    var markerInfo = this.geodata[i];

                    // 构建动态内容
                    var content = `
        <div class="container" style="max-height: 300px; overflow-y: auto;">
          <div class="card">
            <div class="card-body">
              <form>
                <div class="mb-3">
                  <label for="activityName" class="form-label">活动名</label>
                  <input type="text" class="form-control" id="activityName" placeholder="请输入活动名" value="${markerInfo.event_name}">
                </div>
                <div class="mb-3">
                  <label for="activityCount" class="form-label">活动人数</label>
                  <input type="number" class="form-control" id="activityCount" placeholder="请输入活动人数" value="${markerInfo.event_attendees}">
                </div>
                <div class="mb-3">
                  <label for="activityTime" class="form-label">活动时间</label>
                  <input type="text" class="form-control" id="activityTime" placeholder="请输入活动时间" value="${markerInfo.event_time}">
                </div>
              </form>
            </div>
          </div>
        </div>
      `;

                    // 更新InfoWindow的内容
                    infoWindow.setContent(content);

                    // 打开InfoWindow
                    infoWindow.open(this.map, e.target.getPosition());
                });

                // 地图点击事件
                this.map.on('click', () => {
                    this.map.clearInfoWindow();
                });

                // 触发Marker的点击事件
                marker.emit('click', { target: marker });

                this.makerList.push(marker);
            }
        },

        geoCode() {
            var geocoder = new AMap.Geocoder();
            var marker = new AMap.Marker({
                position: null
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
            var that = this;
            geocoder.getLocation(address, function (status, result) {
                if (status === 'complete' && result.geocodes.length) {
                    var lnglat = result.geocodes[0].location

                    // $("#llnglat").value = lnglat;
                    document.getElementById('lnglat').value = lnglat;

                    marker.setPosition(lnglat)
                    that.map.add(marker)
                    that.makerList.push(marker)


                    marker.on('click', (e) => {
                        // infoWindow.setContent(e.target.content);/
                        infoWindow.open(that.map, e.target.getPosition());
                    });



                    marker.emit('click', { target: marker });
                }
                else {
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

        delemarker() {
            for (let i = 0; i < this.makerList.length; i++) {
                this.makerList[i].on('click', (e) => {
                    this.map.remove(this.makerList[i]);
                })
            }

        }

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

#tishi {
    position: absolute;
    bottom: 50px;
    right: 40px;
    z-index: 9;
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