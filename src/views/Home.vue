<template>
  <!-- Checkout this for Search in VUE API Manipulation  -->
  <!-- https://github.com/junip/vue-unsplash/blob/master/src/components/Pictures.vue -->
  <!-- TASK 2 IMPLEMENT The AutoComplete Feature -->

  <v-container fluid>
    <v-row aign="center">
      <v-col cols="12">
        <v-toolbar-title>State Selection</v-toolbar-title>
        <div v-on-clickaway="away" class="searchField dropdown">
          <v-text-field
            :label="labeling"
            v-model="search"
            @input="waitForSearch"
          >
          </v-text-field>
          <!-- <div class="bookParent" v-for="item in items" :key="item.id"> -->
          <!-- <img :src="item.volumeInfo.imageLinks.thumbnail" /> -->
          <HelloWorld v-show="show" v-for="item in items" :key="item.id">
            <template v-slot:contentHandler class="option" id="option1">
              <div class="containerForI">
                <img :src="item.volumeInfo.imageLinks.smallThumbnail" />
                <a
                  @click="updateCard"
                  :title="item.volumeInfo.title"
                  class="anchorTag"
                  >{{ item.volumeInfo.title }}</a
                >
              </div>
            </template>
          </HelloWorld>

          <!-- <img :src="item.volumeInfo.imageLinks.thumbnail" /> -->
          <!-- <a
              @click="updateCard"
              :title="item.volumeInfo.title"
              class="anchorTag"
              href="#"
              v-show="show"
              >{{ item.volumeInfo.title }}</a
            > -->
        </div>
        <div class="content">
          <Content v-for="item in items" :key="item.id" v-show="show2">
            <template v-slot:cardContent>
              <v-card class="mx-auto" elevation="2" outlined shaped>
                <v-list-item three-line>
                  <v-list-item-avatar>
                    <v-img
                      :src="item.volumeInfo.imageLinks.smallThumbnail"
                    ></v-img>
                  </v-list-item-avatar>
                  <v-list-item-content>
                    <v-card-title>
                      {{ item.volumeInfo.title }}
                    </v-card-title>
                    <v-card-subtitle>
                      {{ item.volumeInfo.description }}
                    </v-card-subtitle>
                  </v-list-item-content>
                  <v-card-actions>
                    <v-btn primary>Act</v-btn>
                  </v-card-actions>
                </v-list-item>
              </v-card>
            </template>
          </Content>
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
import HelloWorld from "../components/HelloWorld";
import Content from "../components/Content";
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
      show2: false,
    };
  },
  components: {
    HelloWorld,
    Content,
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
    updateCard() {
      let id = document.querySelector(".anchorTag").getAttribute("title");
      console.log(id);
      this.show = false;
      this.show2 = true;
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
.containerForI {
  /* display: flex; */
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  padding: 20px;
  /* background-color: grey; */
}
.c {
  display: inline-block;
}
.anchorTag {
  /* position: absolute; */
  /* background-color: #f9f9f9; */
  min-width: 160px;
  /* box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2); */
  padding: 12px 16px;
  z-index: 1;
  text-decoration: none;
}
.anchorTag:hover {
  display: block;
}
</style>
<style>
.dropdown .title {
  margin: 0.3em 0.3em 0.3em 0.3em;
  width: 100%;
}

.dropdown .title .fa-angle-right {
  float: right;
  margin-right: 0.7em;
  transition: transform 0.3s;
}

/* .dropdown .menu .option {
  margin: 0.3em 0.3em 0.3em 0.3em;
  margin-top: 0.3em;
} */

.dropdown .menu .option:hover {
  background: rgba(0, 0, 0, 0.2);
}
.pointerCursor:hover {
  cursor: pointer;
}

.rotate-90 {
  transform: rotate(90deg);
}
</style>
