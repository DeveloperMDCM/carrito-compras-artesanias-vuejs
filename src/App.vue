<script setup>
import Header from "./components/Header.vue";
import Guitarra from "./components/Guitarra.vue";
import Footer from "./components/Footer.vue";
import { ref, onMounted, watch } from "vue";
import { db } from "./data/artesanias";

const artesanias = ref([]);
const carrito = ref([]);
const artesania = ref({})

watch(carrito, () => {
  guardarLocalStorage()
}, {
  deep: true
})

onMounted(() => {
  artesanias.value = db;
  artesania.value = db[0]

  const carritoStorage = localStorage.getItem('carrito')
  if(carritoStorage) {
    carrito.value = JSON.parse(carritoStorage)
  }
});

const guardarLocalStorage = () => {
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

const agregarCarrito = (artesania) => {
  const existeCarrito = carrito.value.findIndex(
    (producto) => producto.id === artesania.id
  );
  if (existeCarrito >= 0) {
    if(carrito.value[existeCarrito].quantity <=4){
      carrito.value[existeCarrito].quantity++;
    }
  } else {
    artesania.quantity = 1;
    carrito.value.push(artesania);
  }
};

const incrementarCantidad = (id) => {
  const index = carrito.value.findIndex((producto) => producto.id === id);
  if(carrito.value[index].quantity >= 5) return
  carrito.value[index].quantity++
};
const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex((producto) => producto.id === id);
  if(carrito.value[index].quantity <= 1) return
  carrito.value[index].quantity--;
};

const eliminarProducto = (id) => {
  carrito.value = carrito.value.filter(producto => producto.id !== id)
}

const vaciarCarrito = () => {
  carrito.value = [];
}


</script>
<template>
  <Header
    :carrito="carrito"
    :artesania="artesania"
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestras Artesanias</h2>
    <div class="row mt-5">
      <Guitarra
        :artesania="artesania"
        v-for="artesania in artesanias"
        :key="artesania.id"
        @agregar-carrito="agregarCarrito"
      />
    </div>
  </main>
  <Footer />
</template>

<style scoped></style>
