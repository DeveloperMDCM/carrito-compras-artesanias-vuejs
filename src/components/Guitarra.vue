<script setup>
const props = defineProps({
    artesania: {
        type: Object,
        required: true
    }
})


defineEmits(["agregar-carrito"])

</script>

<template>
    <div class="col-md-6 col-lg-4 my-4 row align-items-center ">
        <div class="col-4">
            <img class="img-fluid" :src="'img/' + artesania.img" :alt="'imagen artesania' + artesania.title">
        </div>
        <div class="col-8 items-home">
            <h3 class="text-black fs-4 fw-bold text-uppercase text-center">{{ artesania.title }}</h3>
            <p class="fw-black fs-3 text-center">{{ artesania.rating }}</p>
            <p>{{ artesania.description }}</p>
            <p class="fw-black text-primary fs-3">${{ artesania.price }}</p>
            <button type="button" class="btn btn-dark w-100 rounded" @click="$emit('agregar-carrito', artesania)">Agregar alCarrito</button>
        </div>
    </div><!-- FIN artesania -->
</template>
