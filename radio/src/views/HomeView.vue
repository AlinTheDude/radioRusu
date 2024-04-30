<template>
  <v-container>
     <h1 class="title">Rusu Radio Browser</h1>
     <div v-if="radios.length">
       <h2>Radio Stations</h2>
       <v-row>
         <v-col cols="12" sm="6" md="4" lg="3" v-for="radio in radios" :key="radio.stationuuid">
           <v-card class="mx-auto" max-width="400" hover dark elevation="10">
             <v-img :src="radio.favicon || require('@/assets/radioImg.jpg')" aspect-ratio="1.75" class="white--text">
               <v-card-title class="fill-height align-end" v-text="radio.name"></v-card-title>
             </v-img>
             <v-card-actions>
               <v-btn color="orange" dark @click="playRadio(radio.url)">
                 <svg-icon type="mdi" :path="mdiPlay"></svg-icon> <!-- Play icon -->
               </v-btn>
               <v-btn color="pink" dark @click="addToFavorites(radio)">
                 <svg-icon type="mdi" :path="mdiHeart"></svg-icon> <!-- Heart icon for favorites -->
               </v-btn>
               <!-- Assuming you want a pause button as well -->
               <v-btn color="red" dark @click="pauseRadio(radio.url)">
                 <svg-icon type="mdi" :path="mdiPause"></svg-icon> <!-- Pause icon -->
               </v-btn>
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
 import SvgIcon from '@jamescoyle/vue-icon';
 import { mdiPlay, mdiPause, mdiHeart } from '@mdi/js';
 
 export default {
  name: 'HomeView',
  components: {
     SvgIcon
  },
  data() {
     return {
       radios: [],
       mdiPlay,
       mdiPause,
       mdiHeart,
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
     pauseRadio(url) {
       // Implement your logic to pause the radio station
       console.log('Pausing radio:', url);
     },
     addToFavorites(radio) {
       // Implement your logic to add the radio station to favorites
       console.log('Adding to favorites:', radio.name);
     },
  },
  created() {
     this.getRadios();
  },
 }
 </script>
 
 <style scoped>
 .title {
  font-family: '70\'s Disco Personal Use', cursive; /* Updated font */
  color: #fff;
  text-align: center;
  margin-bottom: 20px;
 }
 
 .v-card {
  margin-bottom: 20px;
  transition: transform 0.3s ease-in-out;
  background-color: #333; /* Dark background */
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19); /* Shadow for depth */
 }
 
 .v-card:hover {
  transform: scale(1.05);
 }
 
 .v-card-title {
  font-size: 1.2rem;
  font-weight: bold;
  color: #fff; /* White text for better contrast */
 }
 
 .v-card-actions {
  justify-content: center;
 }
 
 .v-btn {
  margin-top: 10px;
 }
 </style>