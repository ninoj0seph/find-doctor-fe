<template>
  <div>
    <v-container>
      <v-row v-for="(row, index) in doctors" :key="index" no-gutters>
        <v-col v-for="(doctor, index) in row" :key="index" style="max-width: 390px; padding: 15px">

          <v-card v-if="doctor.name == ''" class="grey lighten-5" elevation="5" outlined shaped>
            <v-card-title>Create new doctor</v-card-title>
            <v-container>
              <v-text-field v-model="name" placeholder="Name"> </v-text-field>
              <v-autocomplete v-model="value" :items=tags :item-value="value" placeholder="Speciality" multiple chips deletable-chips> </v-autocomplete>
              <v-text-field v-model="address" placeholder="Address"> </v-text-field>
              <v-text-field v-model="location" placeholder="Location"> </v-text-field>
              <v-row class="mb-6" no-gutters>
                <v-col>
                  <v-checkbox
                      v-model="ex4"
                      label="Monday"
                      color="teal accent-4"
                      value="monday"
                      hide-details
                  ></v-checkbox>
                  <v-checkbox
                      v-model="ex4"
                      label="Tuesday"
                      color="teal accent-4"
                      value="tuesday"
                      hide-details
                  ></v-checkbox>
                  <v-checkbox
                      v-model="ex4"
                      label="Wednesday"
                      color="teal accent-4"
                      value="wednesday"
                      hide-details
                  ></v-checkbox>
                  <v-checkbox
                      v-model="ex4"
                      label="Thursday"
                      color="teal accent-4"
                      value="thursday"
                      hide-details
                  ></v-checkbox>
                  <v-checkbox
                      v-model="ex4"
                      label="Friday"
                      color="teal accent-4"
                      value="friday"
                      hide-details
                  ></v-checkbox>
                </v-col>
                <v-col>
                  <v-checkbox
                      v-model="ex4"
                      label="Saturday"
                      color="teal accent-4"
                      value="saturday"
                      hide-details
                  ></v-checkbox>
                  <v-checkbox
                      v-model="ex4"
                      label="Sunday"
                      color="teal accent-4"
                      value="sunday"
                      hide-details
                  ></v-checkbox>
                </v-col>
              </v-row>

            </v-container>

            <v-divider class="mx-4"></v-divider>
            <v-card-actions class="pt-0">
              <v-btn
                  text
                  color="teal accent-4"
                  v-on:click="pushCreateDoctor()"
              >
                Submit
              </v-btn>
            </v-card-actions>
          </v-card>


          <v-card v-else class="grey lighten-5" elevation="5" outlined shaped>
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



        </v-col>
      </v-row>
    </v-container>


  </div>
</template>

<script>

import axios from "axios";


export default {
  name: "AboutView",
  data() {
    return {
      name: null,
      address : null,
      location : null,
      doctors: [],
      create: '',
      tags : [],
      value : [],
      ex4 : [],
    }
  },
  async mounted() {
    await this.fetchDoctors()

  },
  methods : {
    async fetchTags() {
      await axios.get('http://localhost:9090/specialitylookup-all').then(response => {
        this.tags = response.data.map(function (obj) {
          return obj.specialityName
        });
        this.tags = this.tags.sort()
      });
    },
    async fetchDoctors() {
      await this.fetchTags();
      await axios.get('http://localhost:9090/doctors-all').then(response => {
        let createArr = [{
          "name": "",
          "address": "",
          "location": "",
          "schedule": [],
          "speciality": [],
        }];
        this.doctors = this.chunk([...createArr, ...response.data.reverse()], 3)
      });
    },
    chunk(arr, chunkSize) {
      if (chunkSize <= 0) throw "Invalid chunk size";
      let R = [];
      for (let i=0,len=arr.length; i<len; i+=chunkSize)
        R.push(arr.slice(i,i+chunkSize));
      return R;
    },

    async pushCreateDoctor() {
      console.log('c')
      await axios.post('http://localhost:9090/doctor',
  {
        name : this.name,
        address : this.address,
        location : this.location,
        speciality : this.value,
        schedule : this.ex4
      }).then(response => {
        console.log(response);
      });
      this.name = null
        this.address = null
        this.location = null
        this.doctors = []
        this.create = ''
        this.tags = []
        this.value = []
        this.ex4 = []
      await this.fetchDoctors()
    },










  }




}
</script>
