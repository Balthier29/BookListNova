<script setup>
import { ref, reactive, computed } from 'vue';

// 1. Declaración de Props
const props = defineProps({
  libro: {
    type: Object,
    required: true
  },
  index: {
    type: Number,
    required: true
  }
});

// 2. Declaración de Emits
const emit = defineEmits(['eliminar', 'editar']);

// 3. Estado Reactivo (reemplaza data)
const editando = ref(false);
const borrador = reactive({ ...props.libro });

// 4. Propiedades Computadas

const colorEstado = computed(() => {
  if (props.libro.estado === 'Publicado') return 'badge-exito';
  if (props.libro.estado === 'En edición') return 'badge-advertencia';
  return 'badge-info';
});

// 5. Métodos
const activarEdicion = () => {
  Object.assign(borrador, props.libro);
  editando.value = true;
};

const cancelarEdicion = () => {
  editando.value = false;
};

const guardarEdicion = () => {
  emit('editar', { index: props.index, datos: { ...borrador } });
  editando.value = false;
};
</script>

<template>
  <div class="tarjeta-libro">
    <!-- Modo vista -->
    <template v-if="!editando">
      <h3>{{ libro.titulo }}</h3>
      <p class="autor">{{ libro.autor }}</p>
      <div class="badges">
        <span class="badge badge-categoria">{{ libro.categoria }}</span>
        <span class="badge" :class="colorEstado">{{ libro.estado }}</span>
      </div>
      <div class="acciones">
        <router-link :to="{ name: 'detalleLibro', params: { id: index + 1 } }">
          <button type="button">Ver Detalle</button>
        </router-link>
        <button type="button" class="btn-editar" @click="activarEdicion">Editar</button>
        <button type="button" @click.once="emit('eliminar', index)">Eliminar</button>
      </div>
    </template>

    <!-- Modo Edición -->
    <template v-else>
      <div class="campo-editar">
        <label>Título</label>
        <input type="text" v-model="borrador.titulo">
      </div>
      <div class="campo-editar">
        <label>Autor</label>
        <input type="text" v-model="borrador.autor">
      </div>
      <div class="campo-editar">
        <label>Categoría</label>
        <select v-model="borrador.categoria">
          <option value="Novela">Novela</option>
          <option value="Ciencia Ficción">Ciencia Ficción</option>
          <option value="Fantasía">Fantasía</option>
          <option value="Bíografia">Bíografia</option>
          <option value="Historia">Historia</option>
          <option value="Poesía">Poesía</option>
        </select>
      </div>
      <div class="campo-editar">
        <label>Estado</label>
        <select v-model="borrador.estado">
          <option value="En edición">En edición</option>
          <option value="En diseño">En diseño</option>
          <option value="Publicado">Publicado</option>
        </select>
      </div>
      <div class="acciones">
        <button type="button" class="btn-guardar" @click="guardarEdicion">Guardar</button>
        <button type="button" class="btn-secundario" @click="cancelarEdicion">Cancelar</button>
      </div>
    </template>
  </div>
</template>

<style scoped>
.tarjeta-libro {
  background: #FFFFFF;
  border: 1px solid #E2E8F0;
  border-radius: 12px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.tarjeta-libro h3 {
  margin: 0;
  font-size: 15px;
  color: #1A2332;
}

.autor {
  margin: 0;
  font-size: 13px;
  color: #64748B;
}

.badges {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
}

.badge {
  font-size: 11px;
  padding: 3px 10px;
  border-radius: 20px;
}

.badge-categoria {
  background: #E0F2FE;
  color: #1D4ED8;
}

.badge-exito {
  background: #DCFCE7;
  color: #16A34A;
}

.badge-advertencia {
  background: #FEF3C7;
  color: #B45309;
}

.badge-info {
  background: #E0F2FE;
  color: #0369A1;
}

.acciones {
  display: flex;
  gap: 8px;
  margin-top: 8px;
}

.acciones button {
  flex: 1;
  padding: 6px 10px;
  border-radius: 8px;
  font-size: 12px;
  cursor: pointer;
  border: none;
}

.btn-secundario {
  background: #E0F2FE;
  color: #1D4ED8;
}

.btn-peligro {
  background: #F5E2E2;
  color: #A33D3D;
}

.campo-editar {
  display: flex;
  flex-direction: column;
  gap: 3px;
  margin-bottom: 8px;
}

.campo-editar label {
  font-size: 11px;
  color: #64748B;
}

.campo-editar input,
.campo-editar select {
  padding: 6px 8px;
  border: 1px solid #E2E8F0;
  border-radius: 6px;
  font-size: 13px;
}

.btn-editar {
  background: #E0F2FE;
  color: #1D4ED8;
}

.btn-guardar {
  background: #1D4ED8;
  color: white;
}
</style>