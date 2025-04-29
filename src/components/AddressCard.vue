<script lang="ts" setup>
defineProps({
  name: String,
  lastname: String,
  gender: String,
  address: String,
  phone: String,
  cell: String
})
</script>

<template>
  <div class="card mb-4 shadow rounded-2 p-4" dir="rtl">
    <div class="card-body">

      <div class="row mb-4">
        <div class="col">
          <small class="text-body-secondary ">نام</small>
          <p class="card-text">
            <strong>

              {{ name }}
            </strong>
          </p>

        </div>
        <div class="col">
          <small class="text-body-secondary ">نام خانوادگی</small>

          <p class="card-text">
            <strong>

              {{ lastname }}
            </strong>
          </p>
        </div>
        <div class="col">
          <small class="text-body-secondary ">
            شماره
            تلفن همراه
          </small>
          <p class="card-text">
            <strong>

              {{ cell }}
            </strong>
          </p>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <small class="text-body-secondary ">
            شماره
            تلفن ثابت
          </small>
          <p class="card-text">
            <strong>

              {{ phone }}
            </strong>
          </p>

        </div>
        <div class="col">
          <small class="text-body-secondary ">جنسیت</small>

          <p class="card-text">
            <strong>

              {{ gender || '-' }}
            </strong>
          </p>
        </div>
        <div class="col">
          <small class="text-body-secondary ">
            آدرس
          </small>

          <p class="card-text">
            <strong>

              {{ address || '-' }}
            </strong>
          </p>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped>
small {
  color: #b1b1b1;
}

strong {
  color: #6b777c !important;
}
</style>
