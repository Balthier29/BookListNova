<script>
import headLook from './components/headLook.vue';
import portada1984 from '@/assets/img/1984.webp';
import cienAnoSoledad from '@/assets/img/cienAnoSoledad.webp';
import elPrincipito from '@/assets/img/elPrincipito.webp';
import orgullo from '@/assets/img/orgulloYPrejuicio.webp';
import hobbit from '@/assets/img/ElHobbit.webp';
import FooterLook from './components/FooterLook.vue';

export default {
  name: 'app',
  components: {
    headLook,
    FooterLook
  },

  data() {
    return {
      libros: [
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
      ],
      usuarios: [],
      usuarioActivo: null
    }
  },
  methods: {
    agregarLibro(nuevoLibro) {
      const libroID = {
        ...nuevoLibro,
        id: this.libros.length > 0 ? Math.max(...this.libros.map(l => l.id)) + 1 : 1
      };
      this.libros.push(libroID);
    },
    eliminarLibro(index) {
      this.libros.splice(index, 1);
    },
    registrarUsuario(nuevoUsuario) {
      const usuarioID = {
        ...nuevoUsuario,
        id: this.usuarios.length > 0 ? Math.max(...this.usuarios.map(u => u.id)) + 1 : 1
      };
      this.usuarios.push(usuarioID);
    },
    loginUsuario(usuarioEncontrado) {
      this.usuarioActivo = usuarioEncontrado;
      this.$router.push('/libros');
    },
    logout() {
      this.usuarioActivo = null;
      this.$router.push('/');
    },
    editarLibro({index, datos}) {
      this.libros[index] = {...this.libros[index], ...datos};
    }
  }
}


</script>

<template>
  <div>
    <headLook :usuario-activo="usuarioActivo" @logout="logout" />
    <router-view :libros="libros" :usuarios="usuarios" :usuario-activo="usuarioActivo" @agregar-libro="agregarLibro"
    @eliminar-libro="eliminarLibro" 
    @registrar-usuario="registrarUsuario" 
    @login-usuario="loginUsuario" 
    @editar-libro="editarLibro"
    />
    <FooterLook/>
  </div>
</template>

<style></style>