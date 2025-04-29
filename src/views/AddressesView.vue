<script setup lang="ts">
import { onMounted, reactive, ref } from 'vue';
import axios from 'axios'
import LoadingIndicator from '@/components/LoadingIndicator.vue';
import AddressCard from '@/components/AddressCard.vue';

type ResponseType = {
  address: string
address_id: string
coordinate_mobile: string
coordinate_phone_number: string
first_name: string
gender: string
id: string
last_name: string
lat: number
lng: number
}

const error = ref("");
const isLoading = ref(true);
const items = reactive<ResponseType[]>([]);

onMounted (async () => {

  try {
    isLoading.value = true;
    const response = await axios.get<ResponseType[]>('https://stage.achareh.ir/api/karfarmas/address',{
      headers: {
        'Authorization' : 'Basic MDk4MjIyMjIyMjI6U2FuYTEyMzQ1Njc4'
      }
    });
    response.data.forEach((item) => {
      items.push(item);
    });
  } catch (err) {
    error.value = 'Error fetching data:' + String( err);
  }
  isLoading.value = false;

});

</script>

<template>
  <main>
    <div v-if="error" class="alert alert-danger" role="alert">
      {{ error }}
    </div>
    <LoadingIndicator v-if="isLoading" />
    <div class="container" >

      <AddressCard
            v-for="item in items"
            :key="item.id"
            :name="item.first_name"
            :lastname="item.last_name"
            :gender="item.gender"
            :address="item.address"
            :phone="item.coordinate_phone_number"
            :cell="item.coordinate_mobile"
          />
    </div>
  </main>
</template>

<style>

</style>
