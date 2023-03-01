<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col-6 mx-auto">
        <div class="card">
          <div class="card-header"><h2>สมัครสมาชิก</h2></div>
          <div class="card-body">
            <div class="alert alert-danger" role="alert" v-if="firebaseError">
              {{ firebaseError }}
            </div>
            <div class="form-group">
              <label for="email">อีเมล์</label>
              <input v-model="email" class="form-control" type="text" />
            </div>
            <div class="form-group my-3">
              <label for="password">รหัสผ่าน</label>
              <input v-model="password" class="form-control" type="password" />
            </div>
            <div class="form-group my-3">
              <label for="password">รหัสผ่าน</label>
              <input v-model="repassword" class="form-control" type="password" />
            </div>

            <button
              class="btn btn-success d-block mx-auto"
              @click="register"
              :disabled="isBtnLoad"
              type="button"
            >
              {{ btnMessage }}
            </button>
            <div class="text-center">
              <span class="ms-1">ยังไม่มีบัญชี?</span>
              <RouterLink to="/login" class="ms-1" style="text-decoration: none"
                >เข้าสู่ระบบ</RouterLink
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { RouterLink, useRouter } from "vue-router";
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";

const [email, password, repassword] = [ref(""), ref(""), ref("")];
const firebaseError = ref();
const [isBtnLoad, btnMessage] = [ref(false), ref("สมัครสมาชิก")];
const router = useRouter();

function register() {
  if (password.value !== repassword.value) {
    firebaseError.value = "รหัสผ่านไม่ตรงกัน";
    return;
  }
  const auth = getAuth();
  [isBtnLoad.value, btnMessage.value] = [true, "กำลังโหลด...."];
  createUserWithEmailAndPassword(auth, email.value, password.value)
    .then(() => {
      router.push("/login");
    })
    .catch((error) => {
      const errorMessage = error.message;
      [isBtnLoad.value, btnMessage.value] = [false, "สมัครสมาชิก"];
      firebaseError.value = errorMessage;
      // ..
    });
}
</script>
