<template>
  <v-app>

    <v-app-bar
      app
      color="teal accent-4"
      dark
    >
      <RouterLink to="/"  style="text-decoration: none; color: inherit;">
        <div class="d-flex align-center">
          <v-toolbar-title class="text-h6 mr-6 hidden-sm-and-down">
            Find My Doctor - Demo
          </v-toolbar-title>
        </div>
      </RouterLink>
      <v-autocomplete
          v-on:keyup.enter="newWindow(search)"
          v-model="select"
          :loading="loading"
          :items="tags"
          :search-input.sync="search"
          cache-items
          class="mx-4"
          flat
          hide-no-data
          hide-details
          label="Search for a doctor..."
          solo-inverted
      ></v-autocomplete>

      <v-spacer></v-spacer>


      <RouterLink
          style="text-decoration: none; color: inherit;"
          to="/about">
          <v-btn
          href="#"
          target="_blank"
          text>
          <span class="mr-2">Create Doctor</span>
        </v-btn>
      </RouterLink>

      <RouterLink
          style="text-decoration: none; color: inherit;"
          to="/all-tags">
      <v-btn
          target="_blank"
          text
      >
        <span class="mr-2">Manage Tags</span>
      </v-btn>
        </RouterLink>




    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

import axios from "axios";
// import router from "@/router";

export default {
  name: 'App',
  async mounted() {
    await this.fetchTags()
  },

  methods : {
    newWindow(keyword){
      this.$router.push(`/doctor/${keyword}`).catch(()=>{});
    },

    async fetchTags() {
      await axios.get('http://localhost:9090/tagslookup-all').then(response => {
        this.tags = response.data;
      });
    },
  },

  data () {
    return {
      loading: false,
      items: [],
      tags: [],
      search: null,
      select: null,
    }
  },
};
</script>
