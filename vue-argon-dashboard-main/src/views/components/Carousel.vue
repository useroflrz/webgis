<template>
    <div class="card card-carousel overflow-hidden h-100 p-0">
        <div id="carouselExampleCaptions" class="carousel slide h-100">
            <div class="carousel-inner border-radius-lg h-100 data-bs-interval=500">
                <div class="carousel-item h-100 active" :style="{
                    backgroundImage: 'url(' + require('@/assets/img/carousel-1.jpg') + ')',
                    backgroundSize: 'cover'
                }">
                    <div class="carousel-caption d-none d-md-block bottom-0 text-start start-0 ms-5">
                        <div class="icon icon-shape icon-sm bg-white text-center border-radius-md mb-3">
                            <i class="ni ni-camera-compact text-dark opacity-10"></i>
                        </div>
                        <h5 class="text-white mb-1">全国爱心服务热力图</h5>
                        <p>将展示全国地区内使用app进行爱心服务的服务次数</p>
                    </div>
                </div>

                <div ref="myDiv1" class="carousel-item h-100 data-bs-interval=500">
                    <div class="d-title">
                        <h4>全国爱心服务热力图</h4>
                        <h6>全国地区内使用app进行爱心服务的服务次数</h6>
                    </div>
                    <MapContainer ref="myContainer"></MapContainer>
                </div>



                <div class="carousel-item h-100" :style="{
                    backgroundImage: 'url(' + require('@/assets/img/carousel-2.jpg') + ')',
                    backgroundSize: 'cover'
                }">
                    <div class="carousel-caption d-none d-md-block bottom-0 text-start start-0 ms-5">
                        <div class="icon icon-shape icon-sm bg-white text-center border-radius-md mb-3">
                            <i class="ni ni-bulb-61 text-dark opacity-10"></i>
                        </div>
                        <h5 class="text-white mb-1">老人用户及活动分布图</h5>
                        <p>展示老人用户分布情况和正在创办的活动</p>
                    </div>
                </div>

                <div ref="myDiv2" class="carousel-item h-100 data-bs-interval=500">
                    <div class="d-title">
                        <h4>全国爱心服务热力图</h4>
                        <h6>全国地区内使用app进行爱心服务的服务次数</h6>
                    </div>
                    
                    <dis_MapContainer ref="myContainer1"></dis_MapContainer>
                </div>


                <button class="carousel-control-prev w-5 me-3" type="button" data-bs-target="#carouselExampleCaptions"
                    data-bs-slide="prev">
                    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                    <span class="visually-hidden">Previous</span>
                </button>
                <button class="carousel-control-next w-5 me-3" type="button" data-bs-target="#carouselExampleCaptions"
                    data-bs-slide="next">
                    <span class="carousel-control-next-icon" aria-hidden="true"></span>
                    <span class="visually-hidden">Next</span>
                </button>
            </div>

        </div>
    </div>
</template>

<script>
import MapContainer from "./MapContainer.vue";
import dis_MapContainer from "./dis_MapContainer.vue";

export default {
    name: "Carousel",
    mounted() {

        const observer1 = new IntersectionObserver((entries) => {
            entries.forEach((entry) => {
                if (entry.isIntersecting) {
                    // 目标元素进入可视窗口
                    this.$refs.myContainer.initAMap();
                } else {
                    // 目标元素离开可视窗口
                    if (this.$refs.myContainer.map) {
                        this.$refs.myContainer.map.destroy();
                    }
                    // 在这里执行你想要的操作
                }
            });
        });

        const observer2 = new IntersectionObserver((entries) => {
            entries.forEach((entry) => {
                if (entry.isIntersecting) {
                    // 目标元素进入可视窗口
                    this.$refs.myContainer1.initAMap();
                } else {
                    // 目标元素离开可视窗口
                    if (this.$refs.myContainer1.map) {
                        this.$refs.myContainer1.map.destroy();
                    }
                    // 在这里执行你想要的操作
                }
            });
        });

        observer1.observe(this.$refs.myDiv1);
        observer2.observe(this.$refs.myDiv2);

    },
    components: {
        MapContainer,
        dis_MapContainer,
    },
}

</script>

<style scoped>
.d-title {
    position: absolute;
    top: 20px;
    left: 30px;
    z-index: 1;
}
</style>