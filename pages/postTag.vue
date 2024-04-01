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
      <button @click="PostManagerLogin"> loginButton </button>
    </v-container>


    <v-container>
      <label for="id"> getToken :  {{ token }}</label>
    </v-container>


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
      <button @click="PostTag"> postButton </button>
    </v-container>
    </div>
</template>

<script>
export default{
  data:()=>({
    postTagUrl : "https://thirdparty-api.horoli.kr/v1/tag/",
    postMangerSignInUrl : "http://localhost:2017/v1/manager/sign_in",
    selectedType:'PathOfExile',
    statusCode : null,

    id: "",
    password:"",
    token:"",
    
    
    label: "",


  }),
  async mounted(){

  },
  methods:{
    async PostManagerLogin(){
        const postData = await $fetch(
            this.postMangerSignInUrl,{
                method:'POST',
                body:{
                    id: this.id,
                    password : this.password,
                }
            },
        )

        this.token = postData.data.token;

        return postData;
    },
    async PostTag(){
        const postData = await $fetch(
            this.postTagUrl,{
                method:'POST',
                body:{
                    token: this.token,
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