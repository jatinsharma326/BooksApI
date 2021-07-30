<template>
  <!-- Checkout this for Search in VUE API Manipulation  -->
  <!-- https://github.com/junip/vue-unsplash/blob/master/src/components/Pictures.vue -->
  <!-- TASK 2 IMPLEMENT The AutoComplete Feature -->

  <v-container fluid>
    <v-row aign="center">
      <v-col cols="12">
        <v-toolbar-title>State Selection</v-toolbar-title>
        <!-- CheckBox Value -->
        <div v-on-clickaway="away" class="searchField dropdown">
          <v-container fluid>
            <v-row dense>
              <v-col cols="12" class="d-flex flex-wrap justify-space-around">
                <v-checkbox
                  @click="selectCheckbox()"
                  label="intitle"
                  v-model="radioValue"
                  value="intitle"
                >
                </v-checkbox>
                <v-checkbox
                  @click="selectCheckbox()"
                  label="inauthor"
                  v-model="radioValue"
                  value="inauthor"
                >
                </v-checkbox>
                <v-checkbox
                  @click="selectCheckbox()"
                  v-model="radioValue"
                  label="inpublisher"
                  value="inpublisher"
                >
                </v-checkbox>
                <v-checkbox
                  @click="selectCheckbox()"
                  value="subject"
                  label="subject"
                  v-model="radioValue"
                >
                </v-checkbox>
                <v-checkbox
                  @click="selectCheckbox()"
                  value="isbn"
                  label="isbn"
                  v-model="radioValue"
                >
                </v-checkbox>
              </v-col>
            </v-row>
            <v-text-field
              :label="
                `Add
              ${this.radioValue}
              Input`
              "
              v-show="extraTextField"
              v-model="extraInput"
            >
            </v-text-field>
          </v-container>
          <v-text-field
            :label="labeling"
            v-model="search"
            @input="waitForSearch"
          >
          </v-text-field>
          <!-- <div class="bookParent" v-for="item in items" :key="item.id"> -->
          <!-- <img :src="item.volumeInfo.imageLinks.thumbnail" /> -->
          <div class="makelistflex">
            <div
              class="clickUpdateElement "
              v-for="(item, index) in items"
              :key="item.id"
            >
              <HelloWorld v-show="show">
                <template v-slot:contentHandler class="option" id="option1">
                  <div @click="clickCard(item, index)" class="dropdown-content">
                    <v-img style="margin:50px">
                      <img :src="item.volumeInfo.imageLinks.thumbnail" />
                    </v-img>

                    <a class="anchorTag">{{ item.volumeInfo.title }}</a>
                    <!-- <span>{{ item.volumeInfo.author }}</span> -->
                  </div>
                </template>
              </HelloWorld>
            </div>
          </div>
          <div class="content" v-for="(item, index) in items" :key="index">
            <Content v-show="indexToShow === index">
              <template v-slot:cardContent>
                <v-container>
                  <v-row dense>
                    <v-col cols="12">
                      <v-card color="#F8485E">
                        <div class="d-flex flex-no-wrap justify-space-between">
                          <div>
                            <v-card-title class="text-h5 whiteText">{{
                              item.volumeInfo.title
                            }}</v-card-title>

                            <div style="margin-left:45px" class="whiteText">
                              Author:{{ item.volumeInfo.authors }}
                            </div>
                            <v-card-subtitle
                              class="whiteText"
                              style="border-radius:30px"
                              v-text="item.volumeInfo.description"
                            ></v-card-subtitle>
                          </div>

                          <v-avatar class="ma-3" size="200" tile>
                            <v-img
                              :src="item.volumeInfo.imageLinks.thumbnail"
                            ></v-img>
                          </v-avatar>
                        </div>
                      </v-card>
                    </v-col>
                  </v-row>
                </v-container>
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
      // intitle: "",
      // inauthor: "",
      // inpublisher: "",
      // subject: "",
      // isbn: "",
      radioValue: "",
      extraTextField: false,
      extraInput: "",
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
      if (this.search == "") {
        return this.search;
      } else {
        let response = await axios.get(`${this.BASE_URL}`, {
          params: {
            q: `${this.search} ${this.radioValue}:${this.extraInput}`,
            apikey: "",
          },
        });
        // console.log(response.data.items);
        this.items = response.data.items;

        console.log(this.items);
        // fetchAPI();
      }
    },
    away() {
      console.log("clicked away");
      this.show = false;
    },
    clickCard(item, index) {
      console.log(item);
      this.indexToShow = index;
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
    selectCheckbox() {
      if (this.radioValue === "intitle") {
        this.extraTextField = false;
      } else {
        this.extraTextField = true;
      }
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
  max-width: 160px;
  /* box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2); */
  /* padding: 12px 16px; */
  /* z-index: 1; */
  text-decoration: none;
  color: darkmagenta;
  margin-top: 50px;
}

.whiteText {
  color: #eeeeee;
}
.dropdown-content {
  display: -webkit-box;
  /* position: absolute; */
  right: 0;
  background-color: honeydew;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  z-index: 1;
}

.dropdown-content a {
  color: black;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
}

.dropdown-content:hover {
  background-color: #f1f1f1;
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
