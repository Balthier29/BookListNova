<script>
import libroLogin from '@/assets/img/libro-login.gif'

export default {
    name: 'headLook',
    props: {
        usuarioActivo: {
            type: Object,
            default: null
        }
    },
    emits: ['logout'],
    data() {
        return {
            libroLogin
        }
    },
    methods: {
        cerrarSesion() {
            this.$emit('logout')
        }
    }
}
</script>

<template>
    <header>
        <div class="titulo">
            <h1>BookList</h1>
            <p>Tu rincón cultural preferido</p>
        </div>

        <div class="navegacion">
            <nav>
                <ul>
                    <li><router-link to="/" class="link-nav">Inicio</router-link></li>
                    <li><router-link to="/catalogo" class="link-nav">Catalogo</router-link></li>
                    
                    <template v-if="!usuarioActivo">
                        <li><router-link to="/" class="link-nav">Planes</router-link></li>
                        <li><router-link to="/" class="link-nav">Contacto</router-link></li>
                    </template>

                    <template v-else>
                        <li><router-link to="/libros" class="link-nav">libros</router-link></li>
                        <li><router-link to="/libros/1" class="link-nav">detalles</router-link></li>
                    </template>
                </ul>
            </nav>
        </div>

        <div class="login">
            <router-link v-if="!usuarioActivo" to="/registro">
                <button> 🔐Registrar </button>
            </router-link>
            <div v-else class="sesion-activa">
                <span class="bienvenida">Bienvenido/a, {{ usuarioActivo.nombre }}</span>
                <button type="button" @click="cerrarSesion"> Cerrar Sesión</button>
                <span class="icono">
                    <img :src="libroLogin" alt="" width="40">
                </span>
            </div>
        </div>
    </header>
</template>

<style scoped>
header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #1D4ED8;
    color: #FFFFFF;
    padding: 12px 0;

    .titulo {
        display: flex;
        flex-direction: column;
        gap: 2px;
        margin-left: 25px;

        h1 {
            margin: 0;
            font-family: 'Georgia', serif;
            color: #FFFFFF;
        }

        p {
            margin: 0;
            color: #CBD9FB;
            font-size: 13px;
        }
    }

    .navegacion {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;

        nav {
            flex: 1;
        }

        nav ul {
            display: flex;
            justify-content: center;
            list-style: none;
            gap: 20px;
            padding: 0;
            margin: 0;
        }

        nav a.link-nav {
            color: #FFFFFF;
            text-decoration: none;
            font-size: 14px;
            font-weight: 600;
        }
    }

    .login {
        margin: 15px 15px;
        display: flex;
        align-items: center;
        gap: 12px;

        button {
            font-size: 14px;
            border-radius: 8px;
            height: 40px;
            padding: 0 20px;
            border: none;
            background-color: #F97355;
            color: #FFFFFF;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.2s ease;
        }

        button:hover {
            background-color: #F0603F;
        }
    }

    .sesion-activa {
        display: flex;
        align-items: center;
        gap: 12px;
        color: #FFFFFF;
        font-size: 20px;
        font-weight: 600;

        button {
            background-color: transparent;
            border: 1px solid rgba(255, 255, 255, 0.4);
            color: #FFFFFF;
            font-weight: 500;
        }

        button:hover {
            background-color: rgba(255, 255, 255, 0.1);
        }
    }
}
</style>