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
          append-icon="mdi-magnify"
          append-outer-icon
          clearable
          hide-details
          hide-selected
          item-text="name"
          item-value="id"
          label="Search for a doctor..."
          solo-inverted
      >
      </v-autocomplete>

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

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>
