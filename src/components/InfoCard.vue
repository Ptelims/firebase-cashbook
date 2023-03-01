<template>
  <div class="card">
    <div class="card-header d-flex justify-content-between">
      <h5 class="text-success m-0" v-if="props.isIncome">รายรับ จำนวน {{ props.price }} บาท</h5>
      <h5 class="text-danger m-0" v-else>รายจ่าย จำนวน {{ props.price }} บาท</h5>

      <h5 class="m-0">
        {{ dayjs(props.time).format('DD/MM/YYYY') }}
      </h5>
    </div>
    <div class="card-body">
      <h4 v-if="props.title">{{ props.title }}</h4>

      <p>{{ props.description }}</p>
    </div>
  </div>
</template>
<script setup>
import { defineProps } from "vue";
import dayjs from "dayjs"
const props = defineProps({
  title: String,
  price: Number,
  description: String,
  time: String,
  isIncome: Boolean
});
</script>
