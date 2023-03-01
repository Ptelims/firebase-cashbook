<script setup>
import banner from "@/assets/banner.png";
import InfoCard from "@/components/InfoCard.vue";
import { ref } from "vue";

import { initializeApp } from "firebase/app";
import { getFirestore, collection, getDocs } from "firebase/firestore";

const isLoading = ref(true);
const db = getFirestore(initializeApp);
const docRef = collection(db, "Ledger");
const ledger = ref([]);
const getCollections = (docRef) => {
  return new Promise((resolve, reject) => {
    getDocs(docRef)
      .then((collections) => {
        collections.docs.forEach((val) => {
          ledger.value.push(val.data());
          if (val.data().isIncome) {
            totalIncome.value += val.data().price;
          } else {
            totalExpenses.value += val.data().price;
          }
          total.value = totalIncome.value + totalExpenses.value;
        });
        isLoading.value = false;
        resolve(collections.docs);
      })
      .catch((err) => {
        isLoading.value = true;
        alert(err.message);
      }),
      reject;
  });
};
getCollections(docRef);

const totalIncome = ref(0);
const totalExpenses = ref(0);
const total = ref(0);

</script>

<template>
  <main>
    <div class="container">
      <img class="web-banner" :src="banner" alt="" />

      <div class="my-3">
        <div class="row">
          <div class="col-md-3">
            <RouterLink to="/add" class="btn btn-primary w-100" type="button">เพิ่มรายการ</RouterLink>

            <hr />
            <b class="mb-3">รวมสรุปรายการ:</b>
            <div class="d-flex justify-content-between">
              <small>รายรับทั้งหมด</small>
              <small>{{ totalIncome }} บาท</small>
            </div>
            <div class="d-flex justify-content-between">
              <small>รายรายจ่ายทั้งหมด</small>
              <small>{{ totalExpenses }} บาท</small>
            </div>
            <div class="d-flex justify-content-between">
              <small>รวมเป็นเงิน</small>
              <small>{{ total }} บาท</small>
            </div>
          </div>
          <div class="col-md-9">
            <h4 v-if="isLoading">กำลังโหลด...</h4>
            <InfoCard
              :title="stmt.title"
              :price="stmt.price"
              :description="stmt.description"
              :time="stmt.time"
              :isIncome="stmt.isIncome"
              v-for="(stmt, i) in ledger"
              :key="i"
            />
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.web-banner {
  width: 100%;
}
</style>
