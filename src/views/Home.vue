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
          <div
            class="clickUpdateElement"
            v-for="(item, index) in items"
            :key="item.id"
          >
            <HelloWorld v-show="show">
              <template v-slot:contentHandler class="option" id="option1">
                <div @click="clickCard(item, index)" class="containerForI">
                  <v-img>
                    <img :src="item.volumeInfo.imageLinks.thumbnail" />
                  </v-img>

                  <a class="anchorTag">{{ item.volumeInfo.title }}</a>
                  <!-- <span>{{ item.volumeInfo.author }}</span> -->
                </div>
              </template>
            </HelloWorld>
          </div>
          <div class="content" v-for="(item, index) in items" :key="index">
            <Content v-show="indexToShow === null || indexToShow === index">
              <template v-slot:cardContent>
                <v-card class="mx-auto" elevation="2" outlined shaped>
                  <v-list-item three-line>
                    <v-list-item-avatar>
                      <v-img
                        :src="item.volumeInfo.imageLinks.thumbnail"
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
          <!-- <img :src="item.volumeInfo.imageLinks.thumbnail" /> -->
          <!-- <a
              @click="clickCard"
              :title="item.volumeInfo.title"
              class="anchorTag"
              href="#"
              v-show="show"
              >{{ item.volumeInfo.title }}</a
            > -->
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
      indexToShow: null,
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
          apikey: "",
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
    // closeDropdown() {
    //   console.log("Blurr Happening");
    //   this.show = false;
    // },
    clickCard(item, index) {
      // let id = document.querySelector(".anchorTag").getAttribute("title");
      console.log(item);
      this.indexToShow = index;
      // this.imageSrc = target.volumeInfo.imageLinks.thumbnail;
      // this.title = target.volumeInfo.title;
      // this.description = item.volumeInfo.description;
      this.show = false;
    },

    //Method That Take and wait for the Input
    waitForSearch() {
      // this.search = "";
      clearTimeout(this.timerId);

      this.timerId = setTimeout(() => {
        this.getBooks();
      }, 1000);
    },
  },
};
</script>
<style scoped>
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
