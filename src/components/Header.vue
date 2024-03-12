<script setup>    
    import Cart from './Cart.vue';


    const props = defineProps({
        cart: {
            type: Array,
            required: true
        },
        headerGuitar: {
            type: Object,
            required: true
        }
    });

    const emits = defineEmits([
        'show-cart-guitar',
        'decrement-one-guitar',
        'increment-one-guitar',
        'remove-guitar',
        'empty-cart',
        'add-to-cart-header'
    ]);

    const handleShowGuitar = (guitar) => {
        emits('show-cart-guitar', guitar)
    }

    const handleDecrementOneGuitar = (guitarId) => {
        emits('decrement-one-guitar', guitarId)
    }

    const handleIncrementOneGuitar = (guitarId) => {
        emits('increment-one-guitar', guitarId)
    }

    const handleRemoveGuitar = (guitarId) => {
        emits('remove-guitar', guitarId)
    }

    const handleEmptyCart = () => {
        emits('empty-cart')
    }



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
                    <Cart 
                        :cart="cart" 
                        v-on:show-cart-guitar="handleShowGuitar" 
                        v-on:decrement-one-guitar="handleDecrementOneGuitar" 
                        v-on:increment-one-guitar="handleIncrementOneGuitar" 
                        v-on:remove-guitar="handleRemoveGuitar"
                        v-on:empty-cart="handleEmptyCart"
                    />
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">{{ headerGuitar.Name }}</h1>
                    <p class="mt-5 fs-5 text-white">{{ headerGuitar.Description }}</p>
                    <p class="text-primary fs-1 fw-black">${{ headerGuitar.Price }}</p>
                    <button type="button" class="btn fs-4 bg-primary text-white py-2 px-5" v-on:click="$emit('add-to-cart-header', headerGuitar)">Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header">
    </header>

</template>