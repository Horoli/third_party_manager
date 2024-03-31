<template>
    <div>
        {{ getTags }}
    </div>
</template>

<script>
export default {
  data:()=>({
    getTagUrl: "https://thirdparty-api.horoli.kr/v1/tag/",

    getTags :[],

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
}
}

</script>