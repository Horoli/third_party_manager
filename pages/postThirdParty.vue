<template>
  <div>
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
      <label for="mainDescription"> mainDescription : </label>
      <input type="text" id="mainDescription" v-model="mainDescription" />
    </v-container>

    <v-container>
      <label for="subDescription"> subDescription : </label>
      <input type="text" id="subDescription" v-model="subDescription" />
    </v-container>

    <v-container>
      <label for="mainUrl"> mainUrl : </label>
      <input type="text" id="mainUrl" v-model="mainUrl" />
    </v-container>

    <v-container>
      <label for="manualUrl"> manualUrl : </label>
      <input type="text" id="manualUrl" v-model="manualUrl" />
    </v-container>

    <v-container>
      <label for="tags"> tags </label>
      <v-select
        id="tags"
        v-model="selectedTags"
        :items="getTags"
        multiple
      ></v-select>
    </v-container>

    <v-container>
      <label for="image"> image </label>
      <input type="file" id="image" accept="image/*" @change="onImageChange" />
      <img
        :src="imagePreview"
        alt="Image preview"
        v-if="imagePreview"
        class="preview-image"
      />
    </v-container>

    <v-container>
      <button @click="PostThirdParty">post</button>
    </v-container>
  </div>
</template>

<script>
export default {
  data: () => ({
    getTagUrl: "https://thirdparty-api.horoli.kr/v1/tag/",
    postThirdPartyUrl: "https://thirdparty-api.horoli.kr/v1/third_party/",
    postMangerSignInUrl: "https://thirdparty-api.horoli.kr/v1/manager/sign_in",

    selectedType: "PathOfExile",
    statusCode: null,
    getTags: [],

    id: "",
    password: "",
    token: "",

    type: "",
    label: "",
    mainDescription: "",
    subDescription: "",
    mainUrl: "",
    manualUrl: "",
    selectedTags: [],
    base64Image: null,

    imagePreview: null,
  }),
  async mounted() {
    this.getTags = await this.fetchTags();
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

      this.token = postData.data.token;

      return postData;
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

    async PostThirdParty() {
      const postData = await $fetch(this.postThirdPartyUrl, {
        method: "POST",
        body: {
          token: this.token,
          type: this.selectedType,
          label: this.label,
          mainUrl: this.mainUrl,
          manualUrl: this.manualUrl,
          mainDescription: this.mainDescription,
          subDescription: this.subDescription,
          thumbnail: this.base64Image,
          tags: this.selectedTags,
        },
      });

      // this.selectedType = "";
      this.label = "";
      this.mainUrl = "";
      this.mainDescription = "";
      this.subDescription = "";
      this.thumbnail = "";
      this.selectedTags = [];
      this.imagePreview = "";

      return postData;
    },

    onImageChange(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.imagePreview = e.target.result;
        };
        reader.readAsDataURL(file);

        const readerForBase64 = new FileReader();
        readerForBase64.onload = (e) => {
          this.base64Image = e.target.result.split(",")[1];
        };
        readerForBase64.readAsDataURL(file);
      }
    },
  },
};
</script>

<style>
.noneMarkerList {
  list-style: none;
}

.preview-image {
  max-width: 100px;
  max-height: 200px;
  object-fit: contain;
}

.bordered-container {
  border: 1px solid #3e3e3e;
  margin: 10px;
}
</style>
