<template>
  <v-app>
    <v-app-bar color="green">
      <v-app-bar-nav-icon
        ><router-link to="/"
          ><v-icon color="black">mdi-home</v-icon></router-link
        ></v-app-bar-nav-icon
      >
      <!-- <v-toolbar-title>Dates</v-toolbar-title> -->
      <v-spacer></v-spacer>
      <v-btn icon>
        <v-icon>mdi-login</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-pencil</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>
