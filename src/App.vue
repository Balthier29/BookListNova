<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

import headLook from './components/headLook.vue';
import FooterLook from './components/FooterLook.vue';

import portada1984 from '@/assets/img/1984.webp';
import cienAnoSoledad from '@/assets/img/cienAnoSoledad.webp';
import elPrincipito from '@/assets/img/elPrincipito.webp';
import orgullo from '@/assets/img/orgulloYPrejuicio.webp';
import hobbit from '@/assets/img/ElHobbit.webp';

const router = useRouter();


const libros = ref([
  {
    id: 1,
    titulo: 'Cien años de soledad',
    autor: 'Gabriel García Márquez',
    categoria: 'Novela',
    estado: 'Publicado',
    imagen: cienAnoSoledad
  },
  {
    id: 2,
    titulo: '1984',
    autor: 'George Orwell',
    categoria: 'Ciencia Ficción',
    estado: 'Publicado',
    imagen: portada1984
  },
  {
    id: 3,
    titulo: 'El principito',
    autor: 'Antoine de Saint-Exupéry',
    categoria: 'Literatura infantil',
    estado: 'Publicado',
    imagen: elPrincipito
  },
  {
    id: 4,
    titulo: 'Orgullo y prejuicio',
    autor: 'Jane Austen',
    categoria: 'Novela',
    estado: 'En edición',
    imagen: orgullo
  },
  {
    id: 5,
    titulo: 'El hobbit',
    autor: 'J.R.R. Tolkien',
    categoria: 'Fantasía',
    estado: 'En diseño',
    imagen: hobbit
  }
]);

const usuarios = ref([]);
const usuarioActivo = ref(null);

// Métodos
const agregarLibro = (nuevoLibro) => {
  const libroID = {
    ...nuevoLibro,
    id: libros.value.length > 0 ? Math.max(...libros.value.map(l => l.id)) + 1 : 1
  };
  libros.value.push(libroID);
};

const eliminarLibro = (index) => {
  libros.value.splice(index, 1);
};

const registrarUsuario = (nuevoUsuario) => {
  const usuarioID = {
    ...nuevoUsuario,
    id: usuarios.value.length > 0 ? Math.max(...usuarios.value.map(u => u.id)) + 1 : 1
  };
  usuarios.value.push(usuarioID);
};

const loginUsuario = (usuarioEncontrado) => {
  usuarioActivo.value = usuarioEncontrado;
  router.push('/libros');
};

const logout = () => {
  usuarioActivo.value = null;
  router.push('/');
};

const editarLibro = ({ index, datos }) => {
  libros.value[index] = { ...libros.value[index], ...datos };
};
</script>

<template>
  <div>
    <headLook :usuario-activo="usuarioActivo" @logout="logout" />
    <router-view :libros="libros" :usuarios="usuarios" :usuario-activo="usuarioActivo" @agregar-libro="agregarLibro"
      @eliminar-libro="eliminarLibro" @registrar-usuario="registrarUsuario" @login-usuario="loginUsuario"
      @editar-libro="editarLibro" />
    <FooterLook />
  </div>
</template>

<style></style>