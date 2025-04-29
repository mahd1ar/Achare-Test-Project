<script setup lang="ts">
import RegisterGeneralInfo from '@/components/RegisterGeneralInfo.vue'
import RegisterAddress from '@/components/AddressPicker.vue'
import { computed, ref, useTemplateRef } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';


type Data = {
  first_name: string
  last_name: string
  coordinate_mobile: string
  coordinate_phone_number: string
  address: string
  region: string
  lat: number
  lng: number
  gender: 'male' | 'female'
}

const isLoading = ref(false);
const router = useRouter();

const generalInfoTempRef = useTemplateRef('generalInfo');
const location = ref({ lat: 35.7258932, lng: 51.3833876 }); // Default to tehran
const error = ref('');

const data = {} as Data

const step = computed(() => {
  const currentRoute = router.currentRoute.value;
  return currentRoute.query.step ? parseInt(currentRoute.query.step as string, 10) : 2;
});


async function handleNextStep() {
  error.value = ''
  const nextStep = step.value + 1;

  if (step.value === 1) {
    // Validate general info before proceeding to the next step
    if (generalInfoTempRef.value) {
      const result = generalInfoTempRef.value.handleSubmit();
      if (result === false) {
        return; // Prevent proceeding if validation fails
      } else {
        data.address = result.address
        data.first_name = result.firstName
        data.last_name = result.lastName
        data.coordinate_mobile = result.cellPhoneNumber
        data.coordinate_phone_number = result.phoneNumber
        data.gender = result.gender === 'male' ? 'male' : 'female'
      }
    }

    router.push({ query: { ...router.currentRoute.value.query, step: nextStep } });
    return;
  } else if (step.value === 2) {
    try {
      isLoading.value = true;
      await axios.post('https://stage.achareh.ir/api/karfarmas/address', {
        ...data,
        region: '1',
        lat: location.value.lat,
        lng: location.value.lng
      },{
        headers: {
          'Authorization' : 'Basic MDk4MjIyMjIyMjI6U2FuYTEyMzQ1Njc4'
        }
      })
      router.push({ query: { ...router.currentRoute.value.query, step: nextStep } });

    } catch (err) {
      error.value = String(err)
    }
    isLoading.value = false;
  }

}

function handleBackButton() {
  const previousStep = step.value - 1;

  if (previousStep < 1) {
    return;
  }

  router.push({ query: { ...router.currentRoute.value.query, step: previousStep } });
}



</script>

<template>
  <main class="">
    <section class="container-sm">
      <div v-if="error" class="alert alert-danger" role="alert">
        {{ error }}
      </div>
      <div class=" py-3" dir="rtl">
        <button v-if="step === 2" class="border-0 " @click="handleBackButton">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
            <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="m19 12l-6-6m6 6l-6 6m6-6H5" />
          </svg>
          انتخاب آدرس
        </button>
        <div v-else-if="step === 1">
          ثبت آدرس
        </div>
      </div>


      <RegisterGeneralInfo v-show="step === 1" class=" bg-white p-4 rounded-2" ref="generalInfo" />
      <RegisterAddress v-show="step === 2" v-model:modelValue="location" class=" bg-white  rounded-2" />
      <div v-show="step === 3" class="text-center mt-4">

        <div >
          <svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" viewBox="0 0 50 50">
            <path fill="currentColor"
              d="M25 42c-9.4 0-17-7.6-17-17S15.6 8 25 8s17 7.6 17 17s-7.6 17-17 17m0-32c-8.3 0-15 6.7-15 15s6.7 15 15 15s15-6.7 15-15s-6.7-15-15-15" />
            <path fill="currentColor" d="m23 32.4l-8.7-8.7l1.4-1.4l7.3 7.3l11.3-11.3l1.4 1.4z" />
          </svg>
        </div>
        <div class="mb-3" >
          اطلاعات شما با موفقیت ثبت شد
        </div>
        <div>


          <button
          @click="() => {
            router.push('/addresses')
          }"
          type="button" class="btn btn-outline-primary px-4 rounded-1" >
            مشاهده اطلاعات
          </button>

        </div>

      </div>

    </section>


    <footer v-if="step!==3" class="d-flex bg-white flex-wrap justify-content-center align-items-center p-4 border-top">

      <button type="button" :disabled="isLoading" class="btn btn-primary px-5 d-inline-block" @click="handleNextStep">
        <div v-if="isLoading">

          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
            <circle cx="18" cy="12" r="0" fill="currentColor">
              <animate attributeName="r" begin=".67" calcMode="spline" dur="1.5s"
                keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" repeatCount="indefinite"
                values="0;2;0;0" />
            </circle>
            <circle cx="12" cy="12" r="0" fill="currentColor">
              <animate attributeName="r" begin=".33" calcMode="spline" dur="1.5s"
                keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" repeatCount="indefinite"
                values="0;2;0;0" />
            </circle>
            <circle cx="6" cy="12" r="0" fill="currentColor">
              <animate attributeName="r" begin="0" calcMode="spline" dur="1.5s"
                keySplines="0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8;0.2 0.2 0.4 0.8" repeatCount="indefinite"
                values="0;2;0;0" />
            </circle>
          </svg>
        </div>
        <div v-else>

          ثبت نام و ادامه
        </div>
      </button>




    </footer>
  </main>
</template>
