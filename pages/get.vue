<template>
  <div>
    <li v-for="(value, key) in data" :key="key" class="noneMarkerList">
        <li v-for="thirdParty in value">
            <li v-for="(v , k) in thirdParty"> {{ k }} : {{ v }}</li>
        </li>
    </li>
  </div>
</template>

<script>
export default {
  data: () => ({
    url: "http://localhost:4003/v1/third_party/CRAFT",
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
