<template>
  <v-container>
     <h1>Homepage</h1>
     <div v-if="radios.length">
       <h2>Radio Stations</h2>
       <v-row>
         <v-col cols="12" sm="6" md="4" lg="3" v-for="radio in radios" :key="radio.stationuuid">
           <v-card>
             <v-img :src="radio.favicon" aspect-ratio="1.75" class="white--text">
               <v-card-title class="fill-height align-end" v-text="radio.name"></v-card-title>
             </v-img>
             <v-card-actions>
               <v-btn color="orange" dark @click="playRadio(radio.url)">Play</v-btn>
             </v-card-actions>
           </v-card>
         </v-col>
       </v-row>
     </div>
     <div v-else>
       <p>Loading radio stations...</p>
     </div>
  </v-container>
 </template>
 
 <script>
 export default {
  name: 'HomeView',
  data() {
     return {
       radios: [],
     }
  },
  methods: {
     getRadios() {
       fetch('https://nl1.api.radio-browser.info/json/stations/search?limit=100&countrycode=IT&hidebroken=true&order=clickcount&reverse=true')
         .then(response => response.json())
         .then(data => {
           this.radios = data;
           console.log(data);
         });
     },
     playRadio(url) {
       // Implement your logic to play the radio station
       console.log('Playing radio:', url);
     },
  },
  created() {
     this.getRadios();
  },
 }
 </script>
 
 <style scoped>
 .v-card {
  margin-bottom: 20px;
 }
 </style>
 