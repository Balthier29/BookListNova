<script setup>
import { ref, computed } from 'vue';
import FormularioLibro from '@/components/FormularioLibro.vue';
import LibroItem from '@/components/LibroItem.vue';
import ResumenEstado from '@/components/ResumenEstado.vue';

// 1. Props
const props = defineProps({
  libros: {
    type: Array,
    required: true
  }
});

// 2. Emits
const emit = defineEmits(['agregar-libro', 'eliminar-libro', 'editar-libro']);

// 3. Estado Reactivo (data)
const filtroAutor = ref('');
const filtroCategoria = ref('');
const filtroEstado = ref('');
const mostrarFormulario = ref(false);
const mostrarFiltros = ref(false);

// 4. Métodos
const agregarLibro = (nuevoLibro) => {
  emit('agregar-libro', nuevoLibro);
};

const eliminarLibro = (id) => {
  emit('eliminar-libro', id);
};

const editarLibro = (payload) => {
  emit('editar-libro', payload);
};

// 5. Propiedades Computadas
const librosFiltrados = computed(() => {
  return props.libros.filter(libro => {
    const coincideAutor = libro.autor.toLowerCase().includes(filtroAutor.value.toLowerCase());
    const coincideCategoria = filtroCategoria.value === '' || libro.categoria === filtroCategoria.value;
    const coincideEstado = filtroEstado.value === '' || libro.estado === filtroEstado.value;
    return coincideAutor && coincideCategoria && coincideEstado;
  });
});
</script>

<template>
    <div>
        <div class="encabezado-catalogo">
            <h2>Catálogo de libros</h2>
            <p class="subtitulo">Gestiona el catálogo interno de publicaciones.</p>
        </div>

        <ResumenEstado :libros="libros" />

        <div class="acciones-catalogo">
            <button class="btn-toggle" @click="mostrarFormulario = !mostrarFormulario">
                {{ mostrarFormulario ? 'X cerrar formulario' : ' + Agregar Libro' }}
            </button>
            <button class="btn-toggle secundario" @click="mostrarFiltros = !mostrarFiltros">
                {{ mostrarFiltros ? 'X Cerrar Filtros' : '🔍 Filtros' }}
            </button>
        </div>

        <div class="fila-superior" v-if="mostrarFormulario || mostrarFiltros"
            :class="{ 'dos-columnas': mostrarFormulario && mostrarFiltros }">

            <div class="panel-formulario" v-if="mostrarFormulario">
                <FormularioLibro @agregar-libro="agregarLibro" />
            </div>

            <div class="panel-filtros" v-if="mostrarFiltros">
                <h3>Filtros</h3>
                <div class="campo">
                    <label>Buscar por Autor:</label>
                    <input type="text" v-model="filtroAutor" placeholder="Escribe el nombre del autor">
                </div>
                <div class="campo">
                    <label>Filtrar por categoría</label>
                    <select v-model="filtroCategoria">
                        <option value="">Todas las categorías</option>
                        <option value="Novela">Novela</option>
                        <option value="Ciencia Ficción">Ciencia Ficción</option>
                        <option value="Fantasía">Fantasía</option>
                        <option value="Bíografia">Bíografia</option>
                        <option value="Historia">Historia</option>
                        <option value="Poesía">Poesía</option>
                    </select>
                </div>
                <div class="campo">
                    <label>Filtrar por estado</label>
                    <select v-model="filtroEstado">
                        <option value="">Todos los estados</option>
                        <option value="En edición">En edición</option>
                        <option value="En diseño">En diseño</option>
                        <option value="Publicado">Publicado</option>
                    </select>
                </div>
            </div>
        </div>

        <hr>

        <div class="grid-libros" v-if="librosFiltrados.length > 0">
            <LibroItem v-for="libro in librosFiltrados" :key="libros.indexOf(libro)" :libro="libro"
                :index="libros.indexOf(libro)" @eliminar="eliminarLibro" @editar="editarLibro" />
        </div>
        <p v-else>No se encontraron libros con ese criterio de búsqueda.</p>
    </div>
</template>

<style scoped>
.encabezado-catalogo {
    margin-bottom: 20px;
    padding-top: 24px;
}

.encabezado-catalogo h2 {
    margin: 0 0 2px 0;
    font-size: 22px;
    color: #1A2332;
}

.encabezado-catalogo .subtitulo {
    margin: 0;
    font-size: 13px;
    color: #64748B;
}

.acciones-catalogo {
    display: flex;
    gap: 12px;
    margin-bottom: 20px;
}

.btn-toggle {
    background: #1D4ED8;
    color: white;
    border: none;
    padding: 10px 18px;
    border-radius: 8px;
    font-size: 13px;
    font-weight: 600;
    cursor: pointer;
}

.btn-toggle.secundario {
    background: #FFFFFF;
    color: #1D4ED8;
    border: 1px solid #E2E8F0;
}

.btn-toggle:hover {
    opacity: 0.9;
}

.fila-superior {
    display: grid;
    grid-template-columns: 1fr;
    gap: 16px;
    margin-bottom: 20px;
    align-items: start;
}

.fila-superior.dos-columnas {
    grid-template-columns: 1.3fr 1fr;
}

.panel-formulario,
.panel-filtros {
    background: #FAFBFC;
    border: none;
    border-radius: 12px;
    padding: 20px 24px;
}

.panel-filtros h3 {
    margin: 0 0 12px 0;
    font-size: 13px;
    color: #8A94A3;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.5px;
}

.panel-filtros .campo {
    margin-bottom: 12px;
    display: flex;
    flex-direction: column;
    gap: 4px;
}

.panel-filtros label {
    font-size: 12px;
    color: #8A94A3;
    font-weight: 500;
}

.panel-filtros input,
.panel-filtros select {
    padding: 7px 0;
    border: none;
    border-bottom: 1px solid #E3E7ED;
    font-size: 14px;
    background: transparent;
    border-radius: 0;
}

.panel-filtros input:focus,
.panel-filtros select:focus {
    outline: none;
    border-bottom-color: #1D4ED8;
}

.grid-libros {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
    gap: 14px;
}

@media (max-width: 768px) {
    .fila-superior.dos-columnas {
        grid-template-columns: 1fr;
    }

    .acciones-catalogo {
        flex-direction: column;
    }
}
</style>