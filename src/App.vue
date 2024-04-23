<script setup>
    import { ref, reactive, onMounted } from 'vue';

    import Guitarra from "./components/Guitarra.vue"
    import Header from "./components/Header.vue"
    import Footer from "./components/Footer.vue"

    import {db} from "./data/guitarras"

    // Dos formas de declarar
    const guitarras = ref([]);
    const carrito = ref([]);

    // Metodo del ciclo de vida - Componente listo
    onMounted(() => {
      // Se puede hacer una consulta usando fetch o axios
      guitarras.value = db;
    })

    const agregarCarrito = (guitarra) => {
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
    }

</script>

<template>

    <Header
        :carrito="carrito">
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