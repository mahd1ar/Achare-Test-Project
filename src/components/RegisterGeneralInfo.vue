<script setup lang="ts">
import { reactive } from 'vue'

const form = reactive({
  firstName: '',
  lastName: '',
  phoneNumber: '',
  cellPhoneNumber: '',
  gender: '',
  address: '',
})

const errors = reactive({
  firstName: '',
  lastName: '',
  phoneNumber: '',
  cellPhoneNumber: '',
  address: '',
  gender: ''
})

const handleSubmit = () => {
  // Reset errors
  errors.firstName = ''
  errors.lastName = ''
  errors.phoneNumber = ''
  errors.cellPhoneNumber = ''
  errors.address = ''
  errors.gender = ''

  // Validate inputs
  if (form.firstName.length < 3) {
    errors.firstName = 'نام باید حداقل 3 حرف داشته باشد'
    return false
  }
  if (form.lastName.length < 3) {
    errors.lastName = 'نام خانوادگی باید حداقل 3 حرف داشته باشد'
    return false
  }
  if (form.phoneNumber.trim()) {

    if (form.phoneNumber.trim().length !== 11) {
      errors.phoneNumber = "شماره تلفن  ثابت باید 11 رقمی باشد"
      return false
    }
  }

  if (form.cellPhoneNumber.trim())
    if (form.cellPhoneNumber.trim().length !== 11) {
      errors.cellPhoneNumber = ' تلفن همراه باید 11  رقمی باشد '
      return false
    } else if (/^09/.test(form.cellPhoneNumber.trim()) === false) {
      errors.cellPhoneNumber = 'تلفن باید با 09 شروع بشود'
      return false
    }
  if (!form.address) {
    errors.address = 'آدرس نباید خالی باشد'
    return false
  }

  if(form.address.length < 10) {
    errors.address = 'آدرس باید شامل حداقل 10 حرف باشد'
    return false

  }

  if (!form.gender) {
    errors.gender = 'gender is required.'
    return false
  }

  return form

}

defineExpose({
  handleSubmit
})
</script>

<template>
  <div dir="rtl" class="">
    <h5 class=" mb-4">
      لطفا مشخصات و آدرس خود را وارد کنید
    </h5>
    <form @submit.prevent="handleSubmit">

      <div class="row">

        <div class="mb-3 col-lg-4">
          <label for="firstName" class="form-label">نام</label>
          <input type="text" id="firstName" v-model="form.firstName" class="form-control"
            :class="{ 'is-invalid': errors.firstName }" placeholder="مثال: محمدرضا" />
          <div class="invalid-feedback" v-if="errors.firstName">
            {{ errors.firstName }}
          </div>
        </div>

        <div class="mb-3 col-lg-4">
          <label for="lastName" class="form-label">نام خانوادگی</label>
          <input type="text" id="lastName" v-model="form.lastName" class="form-control"
            :class="{ 'is-invalid': errors.lastName }" placeholder="مثال: رضایی" />
          <div class="invalid-feedback" v-if="errors.lastName">
            {{ errors.lastName }}
          </div>
        </div>

        <div class="mb-3 col-lg-4">
          <label for="phoneNumber" class="form-label">شماره تلفن ثابت</label>
          <input type="tel" id="phoneNumber" v-model="form.phoneNumber" class="form-control"
            :class="{ 'is-invalid': errors.phoneNumber }" placeholder="مثال 0912123456" />
          <div class="invalid-feedback" v-if="errors.phoneNumber">
            {{ errors.phoneNumber }}
          </div>
        </div>
      </div>

      <div class="row">

        <div class="mb-3 col-md-4">
          <label for="cellPhoneNumber" class="form-label">شماره تلفن همراه</label>
          <input type="tel" id="cellPhoneNumber" v-model="form.cellPhoneNumber" class="form-control"
            :class="{ 'is-invalid': errors.cellPhoneNumber }" placeholder="مثال 0912123456" />
          <div class="invalid-feedback" v-if="errors.cellPhoneNumber">
            {{ errors.cellPhoneNumber }}
          </div>
        </div>

        <div class="mb-3 col-md-8">
          <label for="address" class="form-label">آدرس</label>
          <input type="tel" id="address" v-model="form.address" class="form-control"
            :class="{ 'is-invalid': errors.address }" placeholder="" />
          <div class="invalid-feedback" v-if="errors.address">
            {{ errors.address }}
          </div>
        </div>
      </div>


      <div class="mb-3">
        <label class="form-label">جنسیت</label>
        <div>
          <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" id="male" value="male" v-model="form.gender" />
            <label class="form-check-label" for="male">آقا</label>
          </div>
          <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" id="female" value="female" v-model="form.gender" />
            <label class="form-check-label" for="female">خانم</label>
          </div>
        </div>
      </div>

      <div class="text-danger" v-if="errors.gender">
        {{ errors.gender }}
      </div>

    </form>

  </div>
</template>


<style scoped></style>
