<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">เพิ่มรายการ</div>
      <div class="card-body">
        <div class="row g-3">
          <div class="col-md-6">
            <div class="form-group">
              <label for="title">ชื่อรายการ</label>
              <input v-model="title" class="form-control" placeholder="ชื่อรายการ" type="text" />
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <label for="isIncome">ประเภท</label>
              <select v-model="isIncome" class="form-select" aria-label="Default select example">
                <option :value="true" select>รายรับ</option>
                <option :value="false">รายจ่าย</option>
              </select>
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <label for="title">จำนวนเงิน</label>
              <input v-model="price" class="form-control" placeholder="ชื่อรายการ" type="number" />
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <label for="title">เวลา</label>
              <input v-model="time" class="form-control" placeholder="เวลา" type="datetime-local" />
            </div>
          </div>
          <div class="col-md-12">
            <div class="form-group">
              <label for="isIncome">ประเภท</label>
              <textarea v-model="description" cols="30" rows="10" class="form-control"></textarea>
            </div>

            <button
              class="btn mt-3 btn-primary w-100"
              type="button"
              @click="saveData"
              :disabled="isBtnLoad"
            >
              {{ btnMessage }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { addDoc, collection, getFirestore } from "@firebase/firestore";
import { initializeApp } from "@firebase/app";
import { useRouter } from "vue-router";

const title = ref("");
const price = ref(0);
const isIncome = ref();
const time = ref("");
const description = ref("");

const db = getFirestore(initializeApp);
const router = useRouter();

const [isBtnLoad, btnMessage] = [ref(false), ref("บันทึกข้อมูล")];

async function saveData() {
  [isBtnLoad.value, btnMessage.value] = [true, "กำลังโหลด...."];
  const docRef = {
    title: title.value,
    price: price.value,
    isIncome: isIncome.value,
    time: time.value,
    description: description.value
  };
  return addDoc(collection(db, "Ledger"), docRef).then(() => {
    router.push("/");
  }).catch(() => {
      [isBtnLoad.value, btnMessage.value] = [false, "บันทึกข้อมูล"];
      alert("Failed to save");
    });
}
</script>
