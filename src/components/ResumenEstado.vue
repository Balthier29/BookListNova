<script>
export default {
    name: 'ResumenEstado',
    props: {
        libros: {
            type: Array,
            required: true
        }
    },
    computed: {
        totalPendientes(){
            return this.contarEstado('En diseño') + this.contarEstado ('En edición');
        },
        totalPublicado(){
            return this.contarEstado('Publicado')
        }
    },
    methods: {
        contarEstado(estado) {
            return this.libros.filter(libro => libro.estado === estado).length;
        }
    }
}
</script>

<template>
    <div class="resumen-wrapper">
        <div class="resumen-estados">
            <span class="chip publicado">📗 Publicados: {{ contarEstado('Publicado') }}</span>
            <span class="chip diseno">🎨 En diseño: {{ contarEstado('En diseño') }}</span>
            <span class="chip edicion">✏️ En edición: {{ contarEstado('En edición') }}</span>
        </div>

        <div class="banner-pendientes" v-if="totalPendientes > 0">
            <span>📋 Pendientes: {{ contarEstado('En diseño') }} en diseño, {{ contarEstado('En edición') }} en edición.</span>
        </div>

        <div class="banner-al-dia" v-else>
            <span>✅ Catálogo al día: {{ totalPublicado }} libros publicados.</span>
        </div>

    </div>
</template>

<style scoped>
.resumen-wrapper {
    margin-bottom: 20px;
}

.resumen-estados {
    display: flex;
    gap: 12px;
    margin-bottom: 12px;
    flex-wrap: wrap;
}

.chip {
    background: #FFFFFF;
    border: 1px solid #E2E8F0;
    padding: 8px 16px;
    border-radius: 20px;
    font-size: 13px;
    color: #1A2332;
}

.banner-pendientes {
    width: 100%;
    box-sizing: border-box;
    background: #B45309;
    color: white;
    padding: 10px 20px;
    border-radius: 8px;
    font-size: 14px;
    font-weight: 600;
}

.banner-al-dia {
    width: 100%;
    box-sizing: border-box;
    background: #16A34A;
    color: white;
    padding: 10px 20px;
    border-radius: 8px;
    font-size: 14px;
    font-weight: 600;
}
</style>