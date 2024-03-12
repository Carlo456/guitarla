<script setup>

import { ref, onMounted, watch } from 'vue';
import { db, total_guitars } from './data/guitarras2';

//Components
import Header from './components/Header.vue';
import Guitar from './components/Guitar.vue';
import Footer from './components/Footer.vue'

const guitars = ref([]);
const cart = ref([]);
const headerGuitar = ref({});
//const number = ref(0);

/*
const state = reactive({
    guitars: []
});
*/

// watch(object_to_watch, callback_function, options)
watch(cart, () => {
    saveCartLocalStorage();
}, {
    deep: true
});

onMounted(() => {
    guitars.value = db;
    //this should be searched by db or with a find to get maybe an special offer, maybe by having an special prop called offer or special to find it
    headerGuitar.value = db.find((guitar) => {
        return guitar.Name === "VAI"
    });
    //console.log("From DB onMounted", headerGuitar);
    //state.guitars = db;
    const cartStorage = localStorage.getItem('cart');
    if (cartStorage) {
        cart.value = JSON.parse(cartStorage);
    }
});    
    /*
    const increment = () => {
        return number.value++;
    }
    */
    const saveCartLocalStorage = () => {
        localStorage.setItem('cart', JSON.stringify(cart.value));    
    }
    const message = (guitar) => {
        console.log(`The guitar from the child: ${JSON.stringify(guitar)}`);
    }

    const addToCart = (guitar) => {
        //cart.value.push(guitar);
        //returns -1 if doesnt exists and the index in array if it exists
        const exists_in_cart = cart.value.findIndex(product => product.Id === guitar.Id);
        if (exists_in_cart >= 0) {
            cart.value[exists_in_cart].quantity++;
        } else {
            guitar.quantity = 1;
            cart.value = [...cart.value, guitar];    
        }
    }

    const addToCartHeader = (guitar) => {
        console.log(guitar);
        addToCart(guitar);
    }

    const decrementOneGuitar = (guitarId) => {
        console.log(`Guitar Id in cart array: ${guitarId}`);
        const cartIndex = cart.value.findIndex(product => product.Id === guitarId);
        if (cart.value[cartIndex].quantity <= 1) {
            console.log("less than cero isnt right")
            return
        }
        cart.value[cartIndex].quantity--;
    }

    const incrementOneGuitar = (guitarId) => {
        console.log(`Added a guitar: ${guitarId}`);
        const cartIndex = cart.value.findIndex(product => product.Id === guitarId);
        if (cart.value[cartIndex].quantity > total_guitars) {
            console.log("having more guitars than the total isnt also right")
        }
        cart.value[cartIndex].quantity++;
    }

    const showCartGuitar = (guitar) => {
        console.log(guitar);
    }

    const emptyCart = () => {
        console.log("removing all objects from cart")
        cart.value = [];
    }

    const removeGuitar = (guitarId) => {
        console.log(guitarId);
        cart.value = cart.value.filter((guitar) => {
            return guitar.Id !== guitarId;
        });
    }

</script>

<template>
    <Header 
        v-bind:cart="cart" 
        v-on:show-cart-guitar="showCartGuitar" 
        v-on:decrement-one-guitar="decrementOneGuitar" 
        v-on:increment-one-guitar="incrementOneGuitar"
        v-on:remove-guitar="removeGuitar" 
        v-on:empty-cart="emptyCart" 
        v-bind:headerGuitar="headerGuitar" 
        v-on:add-to-cart-header="addToCartHeader"
    />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>
        <div class="row mt-5">
            <Guitar @add-to-cart="addToCart" v-for="guitar in guitars" v-bind:guitar="guitar"/>
        </div>
    </main>
    <Footer/>
</template>

<style scoped>

</style>
