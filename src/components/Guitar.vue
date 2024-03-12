<script setup>

    //this is a macro
    const props = defineProps({
        guitar: {
            type: Object,
            required: true
        }
    });
    //this is a macro
    defineEmits([
        //'show-guitar',
        'add-to-cart'
    ]);



</script>
<template>
    <div class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">
            <img class="img-fluid" v-bind:src="`/img/${guitar.PhotoUrl}.jpg`" v-bind:alt="`${guitar.Name ? guitar.Name : 'Default guitar'}`">
        </div>
        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase">{{ guitar.Name }}</h3>
            <p>{{ guitar.Description }}</p>
            <p class="fw-black text-primary fs-3">${{ guitar.Price }}</p>
            <!-- <button v-on:click="$emit('increment')" type="button" class="btn btn-dark w-100 ">Agregar al Carrito</button> -->
            <!-- TO pass info from child to parent just send another param in the $emit function -->
            <!-- <button @click="$emit('show-guitar', guitar)" type="button" class="btn btn-dark w-100 ">Agregar al Carrito</button> -->
            <button @click="$emit('add-to-cart', guitar)" type="button" class="btn btn-dark w-100 ">Add to cart</button>
        </div>
    </div><!-- FIN GUITARRA -->
</template>

<style scoped>
</style>