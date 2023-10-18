<template>
  <div class="ofertas-container" ref="ofertasContainer">
    <div v-for="oferta in ofertasPaginadas" :key="oferta.id" class="oferta-card">
      <div class="imagen-container">
        <img :src="oferta.image" :alt="oferta.title" class="oferta-imagen" />
      </div>
      <div class="oferta-info">
        <h2 class="oferta-nombre">{{ oferta.title }}</h2>
        <p class="oferta-precio">Precio de oferta ${{ oferta.price }}</p>
      </div>
    </div>
    <!-- Controles de paginación -->
    <div class="pagination">
      <button @click="cambiarPagina(paginaActual - 1)" :disabled="paginaActual === 1">Anterior</button>
      <button v-for="numeroPagina in totalPaginas" :key="numeroPagina" @click="cambiarPagina(numeroPagina)" :class="{ active: numeroPagina === paginaActual }">{{ numeroPagina }}</button>
      <button @click="cambiarPagina(paginaActual + 1)" :disabled="paginaActual === totalPaginas">Siguiente</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      ofertas: [],
      elementosPorPagina: 10,
      paginaActual: 1,
    };
  },
  created() {
    this.fetchOfertas();
  },
  computed: {
    totalPaginas() {
      return Math.ceil(this.ofertas.length / this.elementosPorPagina);
    },
    ofertasPaginadas() {
      const inicio = (this.paginaActual - 1) * this.elementosPorPagina;
      const fin = inicio + this.elementosPorPagina;
      return this.ofertas.slice(inicio, fin);
    },
  },
  methods: {
    fetchOfertas() {
      axios.get('https://retoolapi.dev/yASUG4/productos')
        .then(response => {
          this.ofertas = response.data;
        })
        .catch(error => {
          console.error('Error al obtener las ofertas:', error);
        });
    },
    cambiarPagina(pagina) {
      if (pagina >= 1 && pagina <= this.totalPaginas) {
        this.paginaActual = pagina;
        // Hacer scroll hacia arriba
        this.$refs.ofertasContainer.scrollIntoView({ behavior: 'smooth', block: 'start' });
      }
    },
  }
};
</script>

<style scoped>
.ofertas-container {
  display: flex;
  flex-wrap: wrap;
}

.oferta-card {
  border: 1px solid #ccc;
  border-radius: 8px;
  margin: 10px;
  width: 250px;
  box-sizing: border-box;
  background-color: #f1f5f9ff;
  padding: 10px;
  text-align: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Sutil sombra */
}

.oferta-imagen {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  overflow: hidden;
  margin: 0 auto;
}

.oferta-imagen img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 50%;
}

.oferta-nombre {
  font-size: 1.2em;
  margin: 10px 0;
}

.oferta-precio {
  font-size: 1.2em;
  font-weight: bold;
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.pagination button {
  background-color: #007BFF;
  color: #FFFFFF;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  font-size: 1em;
  margin: 0 2px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.pagination button:hover {
  background-color: #0056b3;
}

.pagination button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.pagination .active {
  background-color: #0056b3;
}
</style>
