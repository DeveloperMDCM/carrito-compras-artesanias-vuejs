<script setup>
import { computed } from 'vue';
const props = defineProps({
    carrito: {
        type: Array,
        required: true
    },
    artesania: {
        type: Object,
        required: true
    }
})

defineEmits(["incrementar-cantidad","decrementar-cantidad","agregar-carrito","eliminar-producto","vaciar-carrito"])
const totalPagar = computed(() => {
    return props.carrito.reduce((total, producto) => total + (producto.quantity * producto.price), 0)
})
</script>
<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" width="100" src="/img/header_artesania.png" alt="imagen logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div 
                        class="carrito"
                    >
                        <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />
    
                        <div id="carrito" class="bg-white p-3">
                            <p v-if="carrito.length === 0" class="text-center m-0">El carrito esta vacio</p>
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
                                        <tr v-for="producto in carrito" :key="producto.id">
                                            <td>
                                                <img class="img-fluid" :src="'img/'+ producto.img" :alt="'imagen artesania' + producto.title">
                                            </td>
                                            <td>{{producto.title}}
                                            </td>
                                            <td class="fw-bold text-center">
                                                 <span v-if=" producto.quantity > 1" class="price-independent">{{ producto.quantity }} x {{ producto.price }}</span> {{ producto.price * producto.quantity }}
                                            </td>
                                            <td class="flex align-items-start gap-2">
                                                <button
                                                    type="button"
                                                    class="btn btn-dark rounded"
                                                    @click="$emit('decrementar-cantidad', producto.id)" 
                                                >
                                                    -
                                                </button>
                                                    {{ producto.quantity }}
                                                <button
                                                    type="button"
                                                    class="btn btn-dark rounded"
                                                    @click="$emit('incrementar-cantidad', producto.id)" 
                                                    v-if="producto.quantity <=4"
                                                >
                                                    +
                                                </button>
                                            </td>
                                            <td>
                                                <button
                                                    class="btn btn-danger"
                                                    type="button"
                                                    @click="$emit('eliminar-producto', producto.id)"
                                                >
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                                <p class="text-end">Total pagar: <span class="fw-bold">${{totalPagar}}</span></p>
                                <button class="btn btn-dark w-100 mt-3 p-2 rounded" @click="$emit('vaciar-carrito')">Vaciar Carrito</button>
                            </div>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->
    
            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">{{artesania.title}}</h1>
                    <p class="mt-5 fs-5 text-white">{{ artesania.description }}</p>
                    <p class="text-primary fs-1 fw-black">${{artesania.price}}</p>
                    <button
                        type="button"
                        class="btn fs-4 bg-primary text-white py-2 px-5"
                        @click="$emit('agregar-carrito', artesania)"
                    >Agregar al Carrito</button>
                </div>
            </div>
        </div>
    
        <img class="header-guitarra" src="/img/header_artesania.png" alt="imagen header">
    </header>
</template>