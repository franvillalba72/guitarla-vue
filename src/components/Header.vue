<script setup>
import { computed } from 'vue';

const props = defineProps({
    carrito: {
        type: Array,
        required: true,
    },
    favorita: {
        type: Object,
        required: true,
    }
})

defineEmits([
    'decrementar-cantidad',
    'incrementar-cantidad',
    'eliminar-elemento',
    'vaciar-carrito',
    'agregar-carrito',
])

const totalPagar = computed(() => {
    return props.carrito.reduce((total, item) => total + (item.precio * item.cantidad), 0);
})
</script>

<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="/img/logo.svg" alt="imagen logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div class="carrito">
                        <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />

                        <div id="carrito" class="bg-white p-3">
                            <p class="text-center m-0" v-if="carrito.length === 0">El carrito esta vacio</p>
                            <div v-else>
                                <table class="w-100 table">
                                    <thead>
                                        <tr>
                                            <th>Imagen</th>
                                            <th>Nombre</th>
                                            <th>Precio</th>
                                            <th>Cantidad</th>
                                            <th></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="guitarra in carrito" :key="guitarra.id">
                                            <td>
                                                <img class="img-fluid" :src="`/img/${guitarra.imagen}.jpg`" :alt="`imagen guitarra ${guitarra.nombre}`">
                                            </td>
                                            <td>{{ guitarra.nombre }}</td>
                                            <td class="fw-bold">
                                                ${{ guitarra.precio }}
                                            </td>
                                            <td class="flex align-items-start gap-4">
                                                <button type="button" class="btn btn-dark" @click="$emit('decrementar-cantidad', guitarra.id)">
                                                    -
                                                </button>
                                                {{ guitarra.cantidad }}
                                                <button type="button" class="btn btn-dark" @click="$emit('incrementar-cantidad', guitarra.id)">
                                                    +
                                                </button>
                                            </td>
                                            <td>
                                                <button class="btn btn-danger" type="button" @click="$emit('eliminar-elemento', guitarra.id)">
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>

                                <p class="text-end">Total pagar: <span class="fw-bold">${{ totalPagar }}</span></p>
                                <button class="btn btn-dark w-100 mt-3 p-2" @click="$emit('vaciar-carrito')">Vaciar Carrito</button>
                            </div>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Modelo {{ favorita.nombre }}</h1>
                    <p class="mt-5 fs-5 text-white">{{ favorita.descripcion }}</p>
                    <p class="text-primary fs-1 fw-black">${{ favorita.precio }}</p>
                    <button type="button" class="btn fs-4 bg-primary text-white py-2 px-5" @click="$emit('agregar-carrito', favorita)">Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header">
        <!-- <img class="header-guitarra" :src="`/img/${favorita.imagen}.jpg`" alt="imagen header"> -->
    </header>
</template>