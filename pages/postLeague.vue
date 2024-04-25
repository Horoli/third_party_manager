<template>
<div>
    <v-container>
      <label for="id"> id : </label>
      <input type="text" id="id" v-model="id" />
      <label for="password"> password : </label>
      <input type="password" id="password" v-model="password" />
      <button @click="PostManagerLogin">loginButton</button>
      <label for="id"> getToken : {{ token }}</label>
    </v-container>

    <v-container>
      <label for="label">Label:</label>
      <input type="text" id="label" v-model="label" />

      <label for="version">Version:</label>
      <input type="text" id="version" v-model="version" />
    </v-container>
    <v-container>
      <label for="officialApi">officialApi:</label>
      <input type="text" id="officialApi" v-model="officialApi" />
      </v-container>

    <v-container>
      <label>endState : </label>
      <v-select id="endState" v-model="selectedEndState":items="endStates">
      </v-select>
    </v-container>

    <v-container>
      <label>state : </label>
      <v-select id="state" v-model="selectedState":items="states">
      </v-select>
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
      <label for="start">Start:</label>
      <input type="datetime-local" id="start" v-model="start" />

      <label for="end">End:</label>
      <input type="datetime-local" id="end" v-model="end" />
    </v-container>

    <v-container>
      <button @click="PostLeague">post</button>

    </v-container>
  </div>

</template>

<script>
export default {
  data:()=>({
    // postLeagueUrl: "https://thirdparty-api.horoli.kr/v1/tag/",
    postLeagueUrl: "http://localhost:2017/v1/league/",
    postMangerSignInUrl: "https://thirdparty-api.horoli.kr/v1/manager/sign_in",

    id: "",
    password: "",
    token: "",

    label : "",
    officialApi: "",
    version : "",
    base64Image: null,
    imagePreview: null,
    start :"",
    end:"",
    endStates:['predict', 'confirmed'],
    selectedEndState:"",
    states:['legacy', 'previous', 'current', 'next'],
    selectedState:"",
  }),
  async mounted(){

  },
  methods:{
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

    async PostLeague() {
      const postData = await $fetch(this.postLeagueUrl, {
        method: "POST",
        body: {
          // token: this.token,
          // type: this.selectedType,
          label: this.label,
          version: this.version,
          officialApi: this.officialApi,
          thumbnail: this.base64Image,
          start:this.start,
          end:this.end,
          endState: this.selectedEndState, 
          state:this.selectedState
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

  }
}

</script>