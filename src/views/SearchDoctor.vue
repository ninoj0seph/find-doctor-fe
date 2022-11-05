<template>
  <v-container>
    <v-row v-for="(row, index) in doctors" :key="index" no-gutters>
      <v-col v-for="(doctor, index) in row" :key="index" style="max-width: 390px">
        <v-container style="padding: 15px">
          <v-card  class="grey lighten-5" elevation="5"  outlined shaped>
            <v-card-title>{{doctor.name}}</v-card-title>
            <v-card-subtitle>{{doctor.speciality.join(', ')}}</v-card-subtitle>
            <v-divider class="mx-4"></v-divider>
            <v-card-text>{{doctor.address}} <br> {{doctor.location}} </v-card-text>

            <v-card-subtitle>{{doctor.schedule.join(', ')}}</v-card-subtitle>
            <v-divider class="mx-4"></v-divider>
            <v-card-actions class="pt-0">
              <v-btn
                  text
                  color="teal accent-4"
                  @click="reveal = false"
              >
                Learn More
              </v-btn>
              <v-btn
                  text
                  color="teal accent-4"
                  @click="reveal = false"
              >
                Schedule
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import axios from "axios";

export default {
  name: "SearchDoctor",
  data () {
    return {
      mainTag : null,
      doctors : null,
    }
  },
  watch: {
    async '$route' (to) {
      // console.log(from);
      // console.log();
      await this.getDoctor(to.params.id);
    }
  },
  async mounted() {
    this.mainTag = this.$route.params.id;
    await this.getDoctor(this.mainTag)
  },
  methods : {
    async getDoctor(searchTag){
      await axios.post('http://localhost:9090/doctors', {
        tag : searchTag,
      }).then(response => {
        this.doctors = this.chunk(response.data, 3)
      });

      // window.location.reload();
    },
    chunk(arr, chunkSize) {
      if (chunkSize <= 0) throw "Invalid chunk size";
      let R = [];
      for (let i=0,len=arr.length; i<len; i+=chunkSize)
        R.push(arr.slice(i,i+chunkSize));
      return R;
    },
  }
}
</script>

<style scoped>

</style>