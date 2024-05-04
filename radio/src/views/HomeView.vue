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
                 <v-btn color="orange" dark @click="togglePlayRadio(radio)">
                  <svg-icon type="mdi" :path="currentPlayingUrl === radio.url? mdiPause : mdiPlay"></svg-icon>
                 </v-btn>
                 <v-btn color="pink" dark @click="addToFavorites(radio)">
                  <svg-icon type="mdi" :path="mdiHeart"></svg-icon>
                 </v-btn>
                 <span v-if="currentMediaInfo">{{ currentMediaInfo }}</span>
                 <span v-else>Podcast is playing</span>
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
        currentAudio: null,
        currentPlayingUrl: null,
        currentMediaInfo: null,
        favorites: [],
      };
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
      togglePlayRadio(radio) {
        if (this.currentPlayingUrl === radio.url) {
          this.currentAudio.pause();
          this.currentAudio = null;
          this.currentPlayingUrl = null;
          this.currentMediaInfo = null;
        } else {
          const streamUrl = radio.hls === 1? radio.url : radio.url;
          this.currentAudio = new Audio(streamUrl);
          this.currentAudio.play();
          this.currentPlayingUrl = radio.url;
          this.currentMediaInfo = radio.name;
          console.log('Playing radio:', radio.name);
        }
      },
      addToFavorites(radio) {
        this.favorites.push(radio);
        console.log('Adding to favorites:', radio.name);
        alert(`${radio.name} has been added to favorites.`);
      },
  },
  created() {
      this.getRadios();
  },
 };
 </script>
 
 <style scoped>
 .title {
  font-family: '70\'s Disco Personal Use', cursive;
  color: #fff;
  text-align: center;
  margin-bottom: 20px;
  }
  
 .v-card {
  margin-bottom: 20px;
  transition: transform 0.3s ease-in-out;
  background-color: #333;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19);
  }
  
 .v-card:hover {
  transform: scale(1.05);
  }
  
 .v-card-title {
  font-size: 1.2rem;
  font-weight: bold;
  color: #fff;
  }
  
 .v-card-actions {
  justify-content: center;
  }
  
 .v-btn {
  margin-top: 10px;
  }
 </style>
 