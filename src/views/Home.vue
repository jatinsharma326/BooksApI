<template>
  <!-- Checkout this for Search in VUE API Manipulation  -->
  <!-- https://github.com/junip/vue-unsplash/blob/master/src/components/Pictures.vue -->
  <!-- TASK 2 IMPLEMENT The AutoComplete Feature -->

  <v-container fluid>
    <v-row aign="center">
      <v-col cols="12">
        <v-toolbar-title>State Selection</v-toolbar-title>
        <div v-on-clickaway="away" class="searchField">
          <v-text-field
            :label="labeling"
            v-model="search"
            @input="waitForSearch"
          >
          </v-text-field>
          <div class="bookParent" v-for="item in items" :key="item.id">
            <!-- <img :src="item.volumeInfo.imageLinks.thumbnail" /> -->
            <a class="anchorTag" href="#" v-show="show">{{
              item.volumeInfo.title
            }}</a>
            <!-- @click="changeTheInputTitle" -->
          </div>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
//Click on Anchor It puts on Title
//After SomeTime
//If Person wants another Search fix that issue
//Click Anywhere else in the window it will stop the searching Process - Done
import axios from "axios";
import { directive as onClickaway } from "vue-clickaway";

export default {
  name: "Home",
  directives: {
    onClickaway: onClickaway,
  },
  data() {
    return {
      BASE_URL: "https://www.googleapis.com/books/v1/volumes",
      items: [],
      search: "",
      timerId: "",
      labeling: "Search For Book",
      show: false,
    };
  },
  created() {},
  computed: {},
  //Fetch the required Information from Google Book Api

  watch: {},
  methods: {
    async getBooks() {
      this.show = true;
      let response = await axios.get(`${this.BASE_URL}`, {
        params: {
          q: this.search,
          apikey: "AIzaSyAPAU-uUc7h9mMCAwL2O_8wEoAXKV7CY2w",
        },
      });
      // console.log(response.data.items);
      this.items = response.data.items;

      console.log(this.items);
      // fetchAPI();
    },
    away() {
      console.log("clicked away");
      this.show = false;
    },
    //Method That Take and wait for the Input
    waitForSearch() {
      // this.search = "";
      clearTimeout(this.timerId);

      this.timerId = setTimeout(() => {
        this.getBooks();
      }, 500);
    },
  },
};
</script>
<style scoped>
.bookParent {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  position: relative;
  display: grid;
}
.anchorTag {
  /* position: absolute; */
  background-color: #f9f9f9;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  padding: 12px 16px;
  z-index: 1;
  text-decoration: none;
}
.anchorTag:hover {
  display: block;
}
</style>
