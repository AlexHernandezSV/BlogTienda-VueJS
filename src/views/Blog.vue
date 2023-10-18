<template>
    <br>
    <div id="app">
        <AvisosComponent />
    </div>
    <div id="app">
        <h3>Estas son nuestras increibles ofertas</h3>
      <Ofertas />
    </div>
  </template>
  
  <script>
  import Ofertas from '@/components/Ofertas.vue'
  import AvisosComponent from '@/components/AvisosComponent.vue'
  
  export default {
    components: {
      Ofertas,
      AvisosComponent
    }
  }
  </script>
  
  <style>
  .app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  </style>
  