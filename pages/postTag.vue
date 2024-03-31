<template>
    <div>
    <v-container>
      <label for="type"> type : </label>
      <!-- <input type="text" id="type" v-model="type" /> -->
      <v-select
        id="tags"
        v-model="selectedType"
        :items="['PathOfExile','WorldOfWarcraft']"
      ></v-select>
    </v-container>

    <v-container>
      <label for="label"> label : </label>
      <input type="text" id="label" v-model="label" />
    </v-container>

    <v-container>
      <button @click="PostTag"> post </button>
    </v-container>
    </div>
</template>

<script>
export default{
  data:()=>({
    postTagUrl : "https://thirdparty-api.horoli.kr/v1/tag/",
    selectedType:'PathOfExile',
    statusCode : null,
    
    label: "",


  }),
  async mounted(){

  },
  methods:{
    async PostTag(){
        const postData = await $fetch(
            this.postTagUrl,{
                method:'POST',
                body:{
                    type:this.selectedType,
                    label:this.label,
                }
            }
        )

        this.selectedType = 'PathOfExile'
        this.label = "";


        return postData;
    }
  }
}

</script>