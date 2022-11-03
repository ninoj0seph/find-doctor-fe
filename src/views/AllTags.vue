<template>
  <div>
    <v-container>
      <v-row v-for="(row, indexrow) in tags" :key="indexrow" no-gutters>
        <v-col v-for="(tag, indextag) in row" :key="indextag" style="max-width: 390px; padding: 15px">

          <v-card class="grey lighten-5" elevation="5" v-if="tag.specialityName === 'create'">
              <v-container>
                <v-text-field v-on:keyup.enter="pushCreateSpeciality(tag.txtModel)" v-model="tag.txtModel" placeholder="Create new speciality..."></v-text-field>
              </v-container>
              <br><br><br><br>
              <v-card-actions class="pt-0">
                <v-btn v-on:click="pushCreateSpeciality(tag.txtModel)" text color="teal accent-4">Submit </v-btn>
              </v-card-actions>
          </v-card>

          <v-card class="grey lighten-5" elevation="5" v-else>
            <v-card-title>{{tag.specialityName}}</v-card-title>
            <v-card-subtitle>{{tag.tags.join(', ')}}</v-card-subtitle>
            {{tag.txtModel}}
            <v-container style="padding: 20px">
              <v-text-field v-on:submit="pushTags(tag._id, tag.txtModel)" v-model="tag.txtModel" placeholder="Add new tag..."></v-text-field>
            </v-container>
            <v-card-actions class="pt-0">
              <v-btn v-on:click="pushTags(tag._id, tag.txtModel)" text color="teal accent-4" >Submit </v-btn>
            </v-card-actions>
          </v-card>


        </v-col>
      </v-row>
    </v-container>
  </div>

</template>

<script>
import axios from "axios";

export default {
  name: "AllTags",
  data () {
    return {
      tags: [],
      create : ''
    }
  },
  async mounted() {
    await this.fetchTags()
  },
  methods : {
    async fetchTags() {
      await axios.get('http://localhost:9090/specialitylookup-all').then(response => {
          let createArr = [{specialityName : "create", tags : ['create']}];
          this.tags = this.chunk([...createArr, ...response.data.reverse()], 3)
      });
    },
    async pushCreateSpeciality(spec) {
      console.log('c')
      await axios.post('http://localhost:9090/specialitylookup', {
        specialityName : spec,
        tags : [spec],
      }).then(response => {
        console.log(response);
      });
      await this.fetchTags()
    },
    async pushTags(id, tag) {
      console.log('c')
      await axios.post('http://localhost:9090/addtag', {
        "_id" : id,
        "tags" : tag.split(','),
      }).then(response => {
        console.log(response);
      });
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