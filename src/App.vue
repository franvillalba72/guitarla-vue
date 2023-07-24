<script setup>
import { db } from './data/guitarras.js';
import { ref, onMounted, watch } from 'vue';
import Header from "./components/Header.vue";
import Guitarra from "./components/Guitarra.vue";
import Footer from "./components/Footer.vue";

const guitarras = ref([]);
const carrito = ref([]);
const favorita = ref([]);

watch(carrito, () => {
    guardarLocalStorage();
}, {
    deep: true
})

onMounted(() => {
    guitarras.value = db;
    carrito.value = (JSON.parse(localStorage.getItem('carrito')) || []);
    favorita.value = db[3];
})

const guardarLocalStorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value));
}

const agregarCarrito = (guitarra) => {
    // El método findIndex devuelve el índice de elemento si lo encuentra y -1 si no lo encuentra
    // const existeCarrito = carrito.value.findIndex(item => item.id === guitarra.id);
    // if (existeCarrito >= 0) {
    //     carrito.value[existeCarrito].cantidad++;
    // } else {
    //     guitarra.cantidad = 1;
    //     carrito.value.push(guitarra);
    // }

    if (carrito.value.find(item => item.id === guitarra.id)) {
        guitarra.cantidad++;
    } else {
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
    }
}

const decrementarCantidad = (id) => {
    const indiceElemento = carrito.value.findIndex(item => item.id === id);
    if (carrito.value[indiceElemento].cantidad <= 1) return;
    carrito.value[indiceElemento].cantidad--;
}

const incrementarCantidad = (id) => {
    const indiceElemento = carrito.value.findIndex(item => item.id === id);
    if (carrito.value[indiceElemento].cantidad >= 5) return;
    carrito.value[indiceElemento].cantidad++;
}

const eliminarElemento = (id) => {
    carrito.value = carrito.value.filter(item => item.id !== id);
}

const vaciarCarrito = () => {
    carrito.value = [];
}

</script>

<template>
    <Header 
        :carrito="carrito" 
        :favorita="favorita"
        @decrementar-cantidad="decrementarCantidad" 
        @incrementar-cantidad="incrementarCantidad" 
        @eliminar-elemento="eliminarElemento"
        @vaciar-carrito="vaciarCarrito"
        @agregar-carrito="agregarCarrito" 
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitarra 
                v-for="guitarra in guitarras" 
                :key="guitarra.id" 
                :guitarra="guitarra" 
                @agregar-carrito="agregarCarrito" 
            />
        </div>
    </main>

    <Footer />
</template>