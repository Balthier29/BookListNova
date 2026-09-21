<script>

import libroSinImagen from '@/assets/img/libro-de-lectura.webp'

export default {
    name: 'detalleLibro',
    props: {
        id: {
            type: [String, Number],
            required: true
        },
        libros: {
            type: Array,
            required: true,
            default: () => []
        }
    },
    data() {
        return {
            libroSinImagen
        }
    },
    computed: {
        libro() {
            const posicion = Number(this.id) - 1;
            return this.libros[posicion] || null;
        },
        colorEstado() {
            if (!this.libro) return '';
            if (this.libro.estado === 'Publicado') return 'badge-exito';
            if (this.libro.estado === 'En edición') return 'badge-advertencia';
            return 'badge-info';
        }
    }
}
</script>

<template>
    <div class="pagina-detalle">

        <div class="tarjeta-detalle" v-if="libro">

            <div class="columna-info">
                <div class="badges">
                    <span class="badge badge-categoria">{{ libro.categoria }}</span>
                    <span class="badge" :class="colorEstado">{{ libro.estado }}</span>
                </div>

                <h1>{{ libro.titulo }}</h1>
                <p class="autor">{{ libro.autor }}</p>

                <div class="descripcion">
                    <h3>Descripción</h3>
                    <p>{{ libro.descripcion || 'Sin descripción disponible.' }}</p>
                </div>

                <router-link to="/libros">
                    <button type="button" class="btn-volver">← Volver al listado</button>
                </router-link>
            </div>

            <div class="columna-portada">
                <img v-if="libro.imagen" :src="libro.imagen" :alt="libro.titulo" class="imagen-real">
                <img v-else :src="libroSinImagen" alt="Sin portada disponible" class="sin-imagen">
            </div>

        </div>

        <div v-else class="no-encontrado">
            <p>Libro no encontrado</p>
            <router-link to="/libros">
                <button type="button" class="btn-volver">← Volver al listado</button>
            </router-link>
        </div>

    </div>
</template>

<style scoped>
.pagina-detalle {
    padding: 32px 24px;
    background: #F7F8FA;
    min-height: calc(100vh - 100px);
}

.tarjeta-detalle {
    background: #FFFFFF;
    border: 1px solid #E2E8F0;
    border-radius: 12px;
    max-width: 800px;
    margin: 0 auto;
    padding: 36px;
    display: grid;
    grid-template-columns: 1fr 240px;
    gap: 40px;
    align-items: start;
}

.columna-info {
    display: flex;
    flex-direction: column;
}

.badges {
    display: flex;
    gap: 6px;
    margin-bottom: 14px;
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

.columna-info h1 {
    margin: 0 0 6px 0;
    font-size: 26px;
    color: #1A2332;
}

.autor {
    margin: 0 0 24px 0;
    font-size: 15px;
    color: #64748B;
}

.descripcion h3 {
    font-size: 13px;
    color: #64748B;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    margin-bottom: 8px;
}

.descripcion p {
    font-size: 14px;
    color: #1A2332;
    line-height: 1.7;
    margin-bottom: 28px;
}

.btn-volver {
    background: #1D4ED8;
    color: white;
    border: none;
    padding: 9px 18px;
    border-radius: 8px;
    font-size: 13px;
    font-weight: 500;
    cursor: pointer;
    align-self: flex-start;
}

.btn-volver:hover {
    background: #2563EB;
}

.columna-portada {
    width: 100%;
    display: flex;
    justify-content: center;
}

.imagen-real {
    width: 100%;
    aspect-ratio: 2 / 3;
    border-radius: 10px;
    object-fit: cover;
    box-shadow: 0 8px 24px rgba(26, 35, 50, 0.12);
    display: block;
}

.sin-imagen {
    width: 240px;
    height: 240px;
    border-radius: 10px;
    box-shadow: 0 8px 24px rgba(26, 35, 50, 0.12);
    display: block;
}

.no-encontrado {
    text-align: center;
    padding: 60px 20px;
    color: #64748B;
}
</style>