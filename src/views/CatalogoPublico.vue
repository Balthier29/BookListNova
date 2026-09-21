<script setup>
import { ref, computed } from 'vue';
import libroSinImagen from '@/assets/img/libro-de-lectura.webp';

// 1. Declaración de Props
const props = defineProps({
  libros: {
    type: Array,
    required: true
  }
});

// 2. Estado Reactivo
const indiceActual = ref(0);

// 3. Propiedad Computada
const libroActual = computed(() => props.libros[indiceActual.value]);

// 4. Métodos / Funciones
const anterior = () => {
  // Se corrige "this.indiceActual.length" del código original por "props.libros.length"
  indiceActual.value = indiceActual.value === 0 
    ? props.libros.length - 1 
    : indiceActual.value - 1;
};

const siguiente = () => {
  indiceActual.value = indiceActual.value === props.libros.length - 1 
    ? 0 
    : indiceActual.value + 1;
};

const irA = (i) => {
  indiceActual.value = i;
};
</script>

<template>
  <div class="pagina-catalogo">
    <div class="encabezado-catalogo">
      <h1>Nuestro Catálogo</h1>
      <p>Descubre los títulos publicados por Editorial Nova.</p>
    </div>

    <div class="carrusel" v-if="libros.length > 0">
      <button class="flecha izquierda" @click="anterior">↩️</button>

      <div class="tarjeta-carrusel">
        <img v-if="libroActual.imagen" :src="libroActual.imagen" :alt="libroActual.titulo">
        <img v-else :src="libroSinImagen" alt="sin portada disponible" class="sin-imagen">

        <div class="info-carrusel">
          <span class="badge">{{ libroActual.categoria }}</span>
          <h2>{{ libroActual.titulo }}</h2>
          <p class="autor">{{ libroActual.autor }}</p>
          <p class="descripcion">
            {{ libroActual.descripcion || 'Descubre esta obra en nuestro Catálogo.' }}
          </p>
        </div>
      </div>
      <button class="flecha derecha" @click="siguiente">↪️</button>
    </div>

    <div class="puntos" v-if="libros.length > 0">
      <span 
        v-for="(libro, i) in libros" 
        :key="i" 
        class="punto" 
        :class="{ activo: i === indiceActual }"
        @click="irA(i)"
      ></span>
    </div>
    <p v-else class="vacio">Aún no hay libros publicados en el catálogo</p>
  </div>
</template>

<style scoped>
.pagina-catalogo {
  background: #F7F8FA;
  min-height: calc(100vh - 100px);
  padding: 40px 24px;
}

.encabezado-catalogo {
  text-align: center;
  margin-bottom: 32px;
}

.encabezado-catalogo h1 {
  font-family: 'Georgia', serif;
  color: #1A2332;
  font-size: 28px;
  margin: 0 0 6px 0;
}

.encabezado-catalogo p {
  color: #64748B;
  margin: 0;
}

.carrusel {
  max-width: 800px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  gap: 20px;
}

.tarjeta-carrusel {
  background: #FFFFFF;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(26, 35, 50, 0.1);
  padding: 32px;
  display: grid;
  grid-template-columns: 200px 1fr;
  gap: 28px;
  flex: 1;
  align-items: center;
}

.tarjeta-carrusel img {
  width: 100%;
  aspect-ratio: 2 / 3;
  object-fit: cover;
  border-radius: 10px;
}

.tarjeta-carrusel img.sin-imagen {
  object-fit: contain;
  background: #F4F5F7;
}

.badge {
  display: inline-block;
  background: #E0F2FE;
  color: #0369A1;
  font-size: 11px;
  padding: 3px 10px;
  border-radius: 20px;
  margin-bottom: 10px;
}

.info-carrusel h2 {
  margin: 0 0 4px 0;
  font-size: 22px;
  color: #1A2332;
}

.autor {
  color: #64748B;
  margin: 0 0 14px 0;
}

.descripcion {
  color: #1A2332;
  font-size: 14px;
  line-height: 1.6;
}

.flecha {
  background: #FFFFFF;
  border: 1px solid #E2E8F0;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  font-size: 22px;
  color: #1D4ED8;
  cursor: pointer;
  flex-shrink: 0;
}

.flecha:hover {
  background: #1D4ED8;
  color: white;
}

.puntos {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-top: 24px;
}

.punto {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #CBD5E1;
  cursor: pointer;
}

.punto.activo {
  background: #1D4ED8;
  width: 22px;
  border-radius: 4px;
}

.vacio {
  text-align: center;
  color: #64748B;
}

@media (max-width: 600px) {
  .tarjeta-carrusel {
    grid-template-columns: 1fr;
    text-align: center;
  }
  .tarjeta-carrusel img {
    max-width: 160px;
    margin: 0 auto;
  }
}
</style>