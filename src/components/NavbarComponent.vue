<template>
    <header class="app-header">
      <h3 class="title">Standings</h3>
    </header>
  </template>
  
  <script>
  export default {
    name: "AppNavbar",
  };
  </script>
  
  <style scoped>
  .app-header {
    background-color: hsl(226 71% 25%);
    display: flex; 
    align-items: center; 
    padding: 40px; 
  }
  
  .title {
    color: white;
    margin: 0; 
    text-align: left; 
    flex: 1; 
  }
  </style>
  