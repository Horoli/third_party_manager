<template>
  <div>
    <v-container>
      <div v-for="tag in getTags" :key="tag">
        <button @click="selectTag(tag)">{{ tag }}</button>
      </div>
    </v-container>
    <v-container>
      <label for="id"> id : </label>
      <input type="text" id="id" v-model="id" />
    </v-container>

    <v-container>
      <label for="password"> password : </label>
      <input type="password" id="password" v-model="password" />
    </v-container>

    <v-container>
      <button @click="PostManagerLogin">loginButton</button>
    </v-container>

    <v-container>
      <label for="id"> getToken : {{ token }}</label>
    </v-container>

    <v-container>
      <label for="type"> type : </label>
      <!-- <input type="text" id="type" v-model="type" /> -->
      <v-select
        id="tags"
        v-model="selectedType"
        :items="['PathOfExile', 'WorldOfWarcraft']"
      ></v-select>
    </v-container>

    <v-container>
      <label for="label"> label : </label>
      <input type="text" id="label" v-model="label" />
    </v-container>

    <v-container>
      <button @click="PostTag">postButton</button>
    </v-container>
  </div>
</template>

<script>
export default {
  data: () => ({
    getTags: [],
    getTagUrl: "https://thirdparty-api.horoli.kr/v1/tag/",
    //

    postTagUrl: "https://thirdparty-api.horoli.kr/v1/tag/",
    postMangerSignInUrl: "https://thirdparty-api.horoli.kr/v1/manager/sign_in",
    selectedType: "PathOfExile",
    statusCode: null,

    id: "",
    password: "",
    token: "",

    label: "",
  }),
  async mounted() {
    this.getTags = await this.fetchTags();
  },
  methods: {
    selectTag(tag) {
      this.label = tag;
    },
    async fetchTags() {
      const response = await fetch(this.getTagUrl, {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
      });

      const data = await response.json();
      const filteredTags = data.data.tags.map((tag) => {
        return tag.label;
      });
      return filteredTags;
    },
    async PostManagerLogin() {
      const postData = await $fetch(this.postMangerSignInUrl, {
        method: "POST",
        body: {
          id: this.id,
          password: this.password,
        },
      });

      this.token = postData.data.token;

      return postData;
    },
    async PostTag() {
      const postData = await $fetch(this.postTagUrl, {
        method: "POST",
        body: {
          token: this.token,
          type: this.selectedType,
          label: this.label,
        },
      });

      this.selectedType = "PathOfExile";
      this.label = "";

      return postData;
    },
  },
};
</script>
