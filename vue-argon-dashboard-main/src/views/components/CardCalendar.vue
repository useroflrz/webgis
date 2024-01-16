<template>
  <div class="overflow-hidden card move-on-hover">
    <div class="card-body">
      <div class="d-flex">
        <h6 class="mb-0 me-3">08:00</h6>
        <h6 class="mb-0">
          组会
          <small class="text-secondary font-weight-normal">二楼平台</small>
        </h6>
      </div>
      <hr class="horizontal dark" />
      <div class="d-flex">
        <h6 class="mb-0 me-3">09:30</h6>
        <h6 class="mb-0">
          日常任务 <br />
          <small class="text-secondary font-weight-normal">办公室</small>
        </h6>
      </div>
      <hr class="horizontal dark" />
      <div class="d-flex">
        <h6 class="mb-0 me-3">11:00</h6>
        <h6 class="mb-0">
          设计会议<br />
          <small class="text-secondary font-weight-normal">三楼会议室</small>
        </h6>
      </div>
    </div>
    <a
      href="javascript:;"
      class="py-1 text-center bg-gray-100 w-100"
      data-bs-toggle="tooltip"
      data-bs-placement="top"
      title="Show More"
    >
      <i class="fas fa-chevron-down text-primary"></i>
    </a>
  </div>
</template>

<script>
export default {
  name: "card-calendar",
};
</script>
