<script setup>
import { reactive, computed, ref } from 'vue'

const props = defineProps({
    usuarios: {
        type: Array,
        required: true
    }
})

const emit = defineEmits(['registrar-usuario', 'login-usuario'])

const form = reactive({
    nombreUsuario: '',
    correo: '',
    aceptaCondiciones: '',
})

const enviados = ref(false)

const formularioValido = computed(() =>
    form.nombreUsuario.trim() !== '' && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.correo) && form.aceptaCondiciones
)

function enviar() {
    if (formularioValido.value) {
        emit('registrar-usuario', { nombre: form.nombreUsuario, correo: form.correo })
        enviados.value = true
    }
}

function reiniciar() {
    form.nombreUsuario = '',
    form.correo = '',
    form.aceptaCondiciones= false
    enviados.value = false
}

const correoLogin = ref('')
const errorLogin = ref('')

function intentarLogin() {
    const usuarioEncontrado = props.usuarios.find(u => u.correo === correoLogin.value)

    if (usuarioEncontrado) {
        errorLogin.value = ''
        emit('login-usuario', usuarioEncontrado)
    } else {
        errorLogin.value = 'Usuario no registrado. Verifica tu correo o regístrate'
    }
}

</script>


<template>
    <div class="principal">
        <div class="tarjeta">
            <div v-if="enviados">
                <div class="icono-check">✅</div>
                <h2>Gracias, {{ form.nombreUsuario }}.</h2>
                <p>Favor usar tu mail para ingresar al login</p>
                <button type="button" class="boton-secundario" @click="reiniciar">ir al login</button>
            </div>
            <form v-else @submit.prevent="enviar">
                <h2>Ingresa tus datos para obtener acceso al sistema.</h2>
                <div class="campo">
                    <label> Ingresa tu nombre</label>
                    <input v-model="form.nombreUsuario" type="text" placeholder="Escribe tu Nombre.">
                </div>
                <div class="campo">
                    <label> Ingresa tu Email.</label>
                    <input v-model="form.correo" type="email" placeholder="Escribe tu Email.">
                </div>
                <label class="opcion-checkbox">
                    <input type="checkbox" v-model="form.aceptaCondiciones">
                    <span>Al enviar este formulario, usted acepta la política de <a href="#"
                    @click.prevent>Privacidad</a> </span>
                </label>

                <button type="submit" :disabled="!formularioValido">Enviar</button>
            </form>
        </div>

        <div class="separador" v-show="!enviados"></div>

        <div class="tarjeta" v-show="!enviados">
            <h2>Ingresa tu Email para Acceder</h2>
            <div class="campo">
                <label>Ingresa tu mail para el login</label>
                <input v-model="correoLogin" type="email" @keyup.enter="intentarLogin" placeholder="Ingresa tu Email." >
            </div>
            <p v-if="errorLogin" class="mensaje-error"> {{ errorLogin }}</p>
            <button type="button" @click="intentarLogin" >Acceder</button>

        </div>

    </div>
</template>


<style scoped>
.principal {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 2rem;
    min-height: 10vh;
    margin-top: 5rem;
}

.separador {
    width: 1px;
    align-self: stretch;
    background-color: #E2E8F0;
}

.tarjeta {
    max-width: 480px;
    margin: 2rem auto;
    padding: 2.5rem;
    border-radius: 12px;
    background: #FFFFFF;
    border: 1px solid #E2E8F0;
    font-family: 'Segoe UI', sans-serif;
}

h2 {
    margin-top: 0;
    margin-bottom: 1.8rem;
    color: #1A2332;
    font-size: 1.4rem;
}

.campo {
    margin-bottom: 1.4rem;
    display: flex;
    flex-direction: column;
}

label {
    margin-bottom: 0.5rem;
    font-weight: 600;
    color: #64748B;
    font-size: 0.9rem;
}

input,
select {
    padding: 0.9rem 1rem;
    border: 1px solid #E2E8F0;
    border-radius: 8px;
    font-size: 1rem;
    box-sizing: border-box;
    background: #FAFBFC;
    width: 100%;
    transition: border-color 0.2s, box-shadow 0.2s;
}

input:focus,
select:focus {
    outline: none;
    border-color: #1D4ED8;
    box-shadow: 0 0 0 3px rgba(35, 76, 122, 0.1);
    background: white;
}

.opcion-checkbox {
    display: flex;
    align-items: flex-start;
    gap: 0.6rem;
    margin: 1.2rem 0 1.4rem;
    font-size: 0.85rem;
    color: #64748B;
    cursor: pointer;
    line-height: 1.5;
}

.opcion-checkbox input {
    margin-top: 0.2rem;
    width: 17px;
    height: 17px;
    flex-shrink: 0;
    accent-color: #1D4ED8;
}

.opcion-checkbox a {
    color: #1D4ED8;
    text-decoration: underline;
    font-weight: 600;
}

button {
    width: 100%;
    padding: 0.95rem;
    border: none;
    border-radius: 8px;
    background: #1D4ED8;
    color: white;
    font-weight: bold;
    font-size: 1.05rem;
    cursor: pointer;
    transition: background-color 0.2s;
}

button:disabled {
    background: #C4CCD6;
    cursor: not-allowed;
}

button:not(:disabled):hover {
    background: #2563EB;
}

/* Acento distinto para el botón de LOGIN (segunda tarjeta) */
.tarjeta:last-child button[type="button"] {
    background: #F97355;
}

.tarjeta:last-child button[type="button"]:hover {
    background: #C17B32;
}

.confirmacion {
    text-align: center;
    padding: 1rem 0;
}

.icono-check {
    width: 60px;
    height: 60px;
    margin: 0 auto 1rem;
    border-radius: 50%;
    background: #16A34A;
    color: white;
    font-size: 1.8rem;
    font-weight: bold;
    display: flex;
    align-items: center;
    justify-content: center;
}

.confirmacion h2 {
    margin-bottom: 0.5rem;
}

.confirmacion p {
    color: #64748B;
    line-height: 1.6;
}

.boton-secundario {
    margin-top: 1.3rem;
    padding: 0.7rem 1.3rem;
    border: none;
    border-radius: 8px;
    background: #E0F2FE;
    color: #1D4ED8;
    font-weight: 600;
    cursor: pointer;
}

.boton-secundario:hover {
    background: #E2E8F0;
}

.mensaje-error {
  color: #A33D3D;
  font-size: 0.85rem;
  margin: -0.8rem 0 1rem;
}
</style>