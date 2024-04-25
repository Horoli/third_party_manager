<template>
  <div>
    aaa
  </div>
  <div>
    <li v-for="(value, key) in data" :key="key" class="noneMarkerList" >
        <li v-for="(thirdParty, key) in value">
          <button>
            {{ key }} : {{ thirdParty }}
          </button>
        </li>
    </li>
  </div>
</template>

<script>
export default {
  data: () => ({
    url: "https://thirdparty-api.horoli.kr/v1/third_party/",
    data: [],
  }),
    async created() {
    this.data = await this.fetchData();
  },
  async mounted() {
  },
  methods: {
    async fetchData() {
      const response = await fetch(this.url, {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
      });
      const data = await response.json();
      console.log(data.data);
      return data.data;
    },
  },
};
</script>

<style scoped>


.noneMarkerList {
  list-style: none;
  border: 1px solid #3e3e3e;
  margin: 10px;
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
