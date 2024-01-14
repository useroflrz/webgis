<template>
  <div class="card">
    <div class="card-header pb-0 px-3">
      <h6 class="mb-0">用户名单</h6>
    </div>
    <div class="card-body pt-4 p-3">
      <ul class="list-group">
        <li class="list-group-item border-0 d-flex p-4 mb-2 bg-gray-100 border-radius-lg">
          <div class="d-flex flex-column">
            <h6 class="mb-3 text-sm">张三</h6>
            <span class="mb-2 text-xs">
              用户类型:
              <span class="text-dark font-weight-bold ms-sm-2">爱心用户</span>
            </span>
            <span class="mb-2 text-xs">
              用户电话:
              <span class="text-dark ms-sm-2 font-weight-bold">13245678911</span>
            </span>
            <span class="text-xs">
              服务时长/受服务时长:
              <span class="text-dark ms-sm-2 font-weight-bold">7h</span>
            </span>
          </div>
          <div class="ms-auto text-end">
            <a class="btn btn-link text-danger text-gradient px-3 mb-0" href="javascript:;">
              <i class="far fa-trash-alt me-2" aria-hidden="true"></i>删除
            </a>
            <a class="btn btn-link text-dark px-3 mb-0" href="javascript:;">
              <i class="fas fa-pencil-alt text-dark me-2" aria-hidden="true"></i>编辑
            </a>
          </div>
        </li>
        <li class="list-group-item border-0 d-flex p-4 mb-2 mt-3 bg-gray-100 border-radius-lg">
          <div class="d-flex flex-column">
            <h6 class="mb-3 text-sm">李四</h6>
            <span class="mb-2 text-xs">
              用户类型:
              <span class="text-dark font-weight-bold ms-sm-2">爱心用户</span>
            </span>
            <span class="mb-2 text-xs">
              用户电话:
              <span class="text-dark ms-sm-2 font-weight-bold">18756438899</span>
            </span>
            <span class="text-xs">
              服务时长/受服务时长:
              <span class="text-dark ms-sm-2 font-weight-bold">126h</span>
            </span>
          </div>
          <div class="ms-auto text-end">
            <a class="btn btn-link text-danger text-gradient px-3 mb-0" href="javascript:;">
              <i class="far fa-trash-alt me-2" aria-hidden="true"></i>删除
            </a>
            <a class="btn btn-link text-dark px-3 mb-0" href="javascript:;">
              <i class="fas fa-pencil-alt text-dark me-2" aria-hidden="true"></i>编辑
            </a>
          </div>
        </li>
        <li class="list-group-item border-0 d-flex p-4 mb-2 mt-3 bg-gray-100 border-radius-lg">
          <div class="d-flex flex-column">
            <h6 class="mb-3 text-sm">珍妮</h6>
            <span class="mb-2 text-xs">
              用户类型:
              <span class="text-dark font-weight-bold ms-sm-2">老年人用户</span>
            </span>
            <span class="mb-2 text-xs">
              用户电话:
              <span class="text-dark ms-sm-2 font-weight-bold">15248628697</span>
            </span>
            <span class="text-xs">
              服务时长/受服务时长:
              <span class="text-dark ms-sm-2 font-weight-bold">54h</span>
            </span>
          </div>
          <div class="ms-auto text-end">
            <a class="btn btn-link text-danger text-gradient px-3 mb-0" href="javascript:;">
              <i class="far fa-trash-alt me-2" aria-hidden="true"></i>删除
            </a>
            <a class="btn btn-link text-dark px-3 mb-0" href="javascript:;">
              <i class="fas fa-pencil-alt text-dark me-2" aria-hidden="true"></i>编辑
            </a>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "billing-card",
};
</script>
