<script setup>
    import { ref, reactive, onMounted, watch } from 'vue';

    import Guitarra from "./components/Guitarra.vue"
    import Header from "./components/Header.vue"
    import Footer from "./components/Footer.vue"

    import {db} from "./data/guitarras"

    // Dos formas de declarar
    const guitarras = ref([]);
    const carrito = ref([]);
    const guitarra = ref({});

    // watch -> Ponemos el state que vamos a estar observardo si cambia
    watch(carrito, () => {
        guardarLocalStorage()
    }, {
        deep: true // Para comparar un arreglo de diferentes objetos - verificación estricta
    })

    // Metodo del ciclo de vida - Componente listo
    onMounted(() => {
      // Se puede hacer una consulta usando fetch o axios
      guitarras.value = db;
      guitarra.value = db[3];

      const carritoStorage = localStorage.getItem('carrito');

      if(carritoStorage) {
        carrito.value = JSON.parse(carritoStorage);
      }
    })

    const guardarLocalStorage = () => {
        localStorage.setItem('carrito', JSON.stringify(carrito.value))
    }

    const agregarCarrito = (guitarra) => {

        const existeCarrito = carrito.value.findIndex((producto) => producto.id === guitarra.id)

        if(existeCarrito>=0){
            // carrito.value[existeCarrito].cantidad++
            if(carrito.value[existeCarrito].cantidad>=5) return
            carrito.value[existeCarrito].cantidad++
        }
        else {
            guitarra.cantidad = 1;
            carrito.value.push(guitarra);
        }
    }

    const aumentarProducto = (id) => {
         const index = carrito.value.findIndex((producto) => producto.id === id);
         if(carrito.value[index].cantidad>=5) return
         carrito.value[index].cantidad++;
    }

    const disminuirProducto = (id) => {
         const index = carrito.value.findIndex((producto) => producto.id === id);
         if(carrito.value[index].cantidad<=1) return
         carrito.value[index].cantidad--;
    }

    const eliminarProducto = (id) => {
        // const elemento = carrito.value.findIndex((producto) => producto.id === id);
        // carrito.value.splice(elemento, 1);
        carrito.value = carrito.value.filter(producto => (producto.id !== id))
    }

    const vaciarCarrito = () => {
        carrito.value = []
    }

</script>

<template>

    <Header
        :carrito="carrito"
        :guitarra="guitarra"
        @aumentar-producto="aumentarProducto"
        @disminuir-producto="disminuirProducto"
        @eliminar-producto="eliminarProducto"
        @agregar-carrito="agregarCarrito"
        @vaciar-carrito="vaciarCarrito">
    </Header>

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <!-- Componente Guitarra -->
            <Guitarra
                v-for="guitarra in guitarras"
                :key="guitarra.id"
                v-bind:guitarra="guitarra"
                @agregar-carrito="agregarCarrito"
            ></Guitarra>
        </div>
    </main>

    <Footer/>

</template>