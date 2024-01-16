<template>
  <div class="mt-4 card move-on-hover">
    <div class="card-body">
      <div class="d-flex">
        <p class="mb-0">未读邮件(21)</p>
        <a
          href="javascript:;"
          class="ms-auto"
          data-bs-toggle="tooltip"
          data-bs-placement="top"
          title="Check your emails"
        >
          查看
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "card-email",
};
</script>
