<template>
  <div>
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
      <v-select id="endState" v-model="selectedEndState" :items="endStates">
      </v-select>
    </v-container>

    <v-container>
      <label>state : </label>
      <v-select id="state" v-model="selectedState" :items="states"> </v-select>
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
  data: () => ({
    // postLeagueUrl: "https://thirdparty-api.horoli.kr/v1/league/",
    postLeagueUrl: "http://localhost:2017/v1/league/",
    postMangerSignInUrl: "https://thirdparty-api.horoli.kr/v1/manager/sign_in",

    label: "",
    officialApi: "",
    version: "",
    start: "",
    end: "",
    endStates: ["predict", "confirmed"],
    selectedEndState: "",
    states: ["legacy", "previous", "current", "next"],
    selectedState: "",
  }),
  async mounted() {},
  inject: ["managerToken"],
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

    async PostLeague() {
      const postData = await $fetch(this.postLeagueUrl, {
        method: "POST",
        body: {
          token: this.managerToken.value,
          label: this.label,
          version: this.version,
          officialApi: this.officialApi,
          thumbnail: this.base64Image,
          start: this.start,
          end: this.end,
          endState: this.selectedEndState,
          state: this.selectedState,
        },
      });

      this.label = "";
      this.mainUrl = "";
      this.mainDescription = "";
      this.subDescription = "";
      this.thumbnail = "";
      this.selectedTags = [];
      this.imagePreview = "";

      return postData;
    },
  },
};
</script>
