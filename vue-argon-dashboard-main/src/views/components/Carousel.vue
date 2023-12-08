<template>
    <div class="card card-carousel overflow-hidden h-100 p-0">
        <div id="carouselExampleCaptions" class="carousel slide h-100">
            <div class="carousel-inner border-radius-lg h-100">
                <div ref="myDiv" class="carousel-item h-100 active data-bs-interval=100000000000">
                    <MapContainer ref="myContainer"></MapContainer>
                </div>
                <div class="carousel-item h-100 data-bs-interval=100000000000">
                    轮播1
                </div>
                <div class="carousel-item h-100 data-bs-interval=100000000000">
                    轮播2
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
import MapContainer from "./MapContainer.vue"


export default {
    name: "Carousel",
    mounted() {

        const observer = new IntersectionObserver((entries) => {
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

        observer.observe(this.$refs.myDiv);
    },
    components: {
        MapContainer,
    },
}

</script>