<template>
  <div>
    <div v-if="avisos.length > 0">
      <transition name="fade" mode="out-in" style="background-color: #4caf50;">
        <div :key="currentAviso.id" class="alert">
          <h2 style="color: white;">{{ currentAviso.titulo }}</h2>
          <p style="color: white;">{{ currentAviso.Cuerpo }}</p>
        </div>
      </transition>
    </div>
    <div v-else>
      <h5>Cargando avisos...</h5>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      avisos: [],
      currentAvisoIndex: 0,
    };
  },
  computed: {
    currentAviso() {
      return this.avisos[this.currentAvisoIndex];
    },
  },
  mounted() {
    this.getAvisos();
    setInterval(this.changeAviso, 5000); // Cambia el aviso cada 5 segundos
  },
  methods: {
    getAvisos() {
      axios.get('https://retoolapi.dev/WfzJwb/avisos')
        .then(response => {
          this.avisos = response.data;
        })
        .catch(error => {
          console.error(error);
        });
    },
    changeAviso() {
      this.currentAvisoIndex = (this.currentAvisoIndex + 1) % this.avisos.length;
    },
  },
};
</script>

<style>
.alert {
  background-color: #4caf50; /* Un color verde brillante */
  color: white; /* Texto en blanco */
  padding: 1rem;
  margin-bottom: 1rem;
  font-family: 'Arial', sans-serif; /* Cambiamos la fuente a Arial */
  border-radius: 8px; /* Añadimos bordes redondeados */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Sutil sombra */
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.8s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
