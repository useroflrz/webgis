<template>
  <div v-show="this.$store.state.layout !== 'landing'" class="card card-plain shadow-none" id="sidenavCard">
    <div class="p-3 card-body text-center w-100 pt-0">
      <img class="w-50 mx-auto" src="@/assets/img/illustrations/icon-documentation.svg" alt="sidebar_illustration" />
      <h6 v-if="this.$store.state.isRTL" class="mb-0 text-dark up">
      </h6>
      <h6 v-else class="mb-0 text-dark up">需要帮助？</h6>
      <p v-if="this.$store.state.isRTL" class="text-xs font-weight-bold">
      </p>
      <p v-else class="text-xs font-weight-bold">Please check our docs</p>
    </div>
    <a v-if="this.$store.state.isRTL" href="" target="_blank" class="mb-3 btn btn-dark btn-sm w-100">توثيق</a>
    <a v-else href="../../../README.md" class="mb-3 btn btn-dark btn-sm w-100">文件</a>
    <a v-if="this.$store.state.isRTL" href="" target="_blank" class="mb-3 btn btn-success btn-sm w-100"></a>

    <a v-else href="" target="_blank" class="mb-3 btn btn-success btn-sm w-100">刷新</a>
  </div>
</template>
<script>
import img from "../../assets/img/illustrations/icon-documentation.svg";

export default {
  name: "sidenav-card",
  props: ["textPrimary", "textSecondary", "iconClass"],
  data() {
    return {
      img
    };
  }
};
</script>
