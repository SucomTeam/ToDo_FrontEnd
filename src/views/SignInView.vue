<template>
  <v-card class="mx-auto px-6 py-8" max-width="344" title="로그인">
    <v-form v-model="form" @submit.prevent="onSubmit">
      <v-text-field
        v-model="username"
        :rules="[username_rule]"
        class="mb-2"
        clearable
        label="아이디"
      ></v-text-field>

      <v-text-field
        v-model="password"
        type="password"
        :rules="[password_rule]"
        clearable
        label="비밀번호"
      ></v-text-field>

      <br />

      <v-btn
        :disabled="!form"
        block
        color="success"
        size="large"
        type="submit"
        variant="elevated"
      >
        로그인
      </v-btn>
    </v-form>
  </v-card>
</template>

<script>
import axios from "axios";
import { ref } from "vue";
import { useUserStore } from "@/stores/user";
import { useRouter } from "vue-router";

export default {
  setup() {
    const form = ref(false);
    const username = ref("");
    const password = ref("");

    const userStore = useUserStore();

    const router = useRouter();

    const onSubmit = () => {
      if (!form.value) return;

      const loginData = {
        username: username.value,
        password: password.value,
      };

      // API 요청 구간
      axios
        .post("/api/v1/member/signin", loginData)
        .then((res) => {
          console.log(res.data);
          userStore.login(res.data);
          router.push("/");
        })
        .catch((err) => {
          console.log(err.response.data.message);
        });
    };

    return {
      form,
      username,
      password,
      onSubmit,
      username_rule(v) {
        return !!v || "아이디를 입력하세요.";
      },
      password_rule(v) {
        return !!v || "비밀번호를 입력하세요.";
      },
    };
  },
};
</script>

<style></style>
