<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link> |
    <router-link to="/contatti">Contatti</router-link>
  </nav>
  <router-view/>
</template>

<style>
#app {
 font-family: 'Roboto Mono', monospace;
 -webkit-font-smoothing: antialiased;
 -moz-osx-font-smoothing: grayscale;
 text-align: center;
 color: #fff; /* Changed to white for better contrast against the dark background */
 background-color: #333; /* Dark background */
}

nav {
 padding: 30px;
 background-color: #444; /* Slightly lighter background for the nav */
}

nav a {
 font-weight: bold;
 color: #fff; /* Changed to white for better contrast against the dark background */
}

nav a.router-link-exact-active {
 color: #42b983; /* Keeping the active link color as it is */
}
</style>
