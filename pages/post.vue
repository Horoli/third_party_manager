<template>
  <div>
    <v-container>
      <label for="type"> type : </label>
      <input type="text" id="type" v-model="type" />
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
      <button @click="PostThirdParty"> post </button>
    </v-container>
  </div>
</template>

<script>
export default {
  data: () => ({
    getTagUrl: "http://localhost:4003/v1/tag/",
    postThirdPartyUrl: "http://localhost:4003/v1/third_party/",

    statusCode: null,
    getTags: [],
    type: "",
    label: "",
    mainDescription: "",
    subDescription: "",
    mainUrl: "",
    selectedTags: [],
    base64Image: null,

    imagePreview: null,
  }),
  async mounted() {
    this.getTags = await this.fetchTags();
  },
  methods: {
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

    async PostThirdParty(){
      const postData = await $fetch(this.postThirdPartyUrl, {
        method:'POST',
        body:{
          type:this.type,
          label: this.label,
          mainUrl: this.mainUrl,
          mainDescription: this.mainDescription,
          subDescription: this.subDescription,
          thumbnail: this.base64Image,
          tags:this.selectedTags,
        }
      })

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
