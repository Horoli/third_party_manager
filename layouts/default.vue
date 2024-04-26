<template>
  <div>
    <AppNavigationBar />
    <v-container>
      <label for="id"> id : </label>
      <input type="text" id="id" v-model="id" />
      <label for="password"> password : </label>
      <input type="password" id="password" v-model="password" />
      <button @click="PostManagerLogin">loginButton</button>
      <label for="id"> getToken : {{ token.value }}</label>
    </v-container>

    <slot />
    <AppFooter />
  </div>
</template>

<script>
export default {
  data: () => ({
    postMangerSignInUrl: "https://thirdparty-api.horoli.kr/v1/manager/sign_in",

    id: "",
    password: "",
    token: {
      value: "",
    },
  }),
  provide() {
    return {
      managerToken: this.token,
    };
  },
  methods: {
    async PostManagerLogin() {
      const postData = await $fetch(this.postMangerSignInUrl, {
        method: "POST",
        body: {
          id: this.id,
          password: this.password,
        },
      });
      this.token.value = postData.data.token;
      return postData;
    },
  },
};
</script>
