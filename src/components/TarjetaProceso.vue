<script setup>

defineProps({
  icono: {
    type: String,
    required: true
  },
  titulo: {
    type: String,
    required: true
  },
  descripcion: {
    type: String,
    required: true
  }
});
</script>

<template>
  <div class="tarjeta-proceso">
    <img :src="icono" :alt="titulo" class="icono-proceso">
    <h3>{{ titulo }}</h3>
    <p>{{ descripcion }}</p>
  </div>
</template>

<style scoped>
.tarjeta-proceso {
  text-align: center;
  padding: 20px;
}

.icono-proceso {
  width: 64px;
  height: 64px;
  margin-bottom: 14px;
}

.tarjeta-proceso h3 {
  font-size: 17px;
  color: #1A2332;
  margin: 0 0 10px 0;
  font-family: 'Georgia', serif;
}

.tarjeta-proceso p {
  font-size: 13px;
  color: #64748B;
  line-height: 1.6;
  margin: 0;
}
</style>