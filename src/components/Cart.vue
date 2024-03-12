<script setup>
    import { computed } from 'vue';
    
    const props = defineProps({
        cart: {
            type: Array,
            required: true
        }
    });

    defineEmits([
        'show-cart-guitar',
        'decrement-one-guitar',
        'increment-one-guitar',
        'remove-guitar',
        'empty-cart'
    ]);

    const calculateTotalCartPrice = computed(() => {
        //need to use an array reduce to calculate
        return props.cart.reduce((total_price, current_guitar_price) => total_price + (current_guitar_price.quantity * current_guitar_price.Price ), 0);
    });
    
</script>
<template>
    <div class="carrito">
        <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />
        <div id="carrito" class="bg-white p-3">
            <p v-if="cart.length === 0" class="text-center">El carrito esta vacio</p>
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
                    <tr v-for="guitar in cart">
                        <td>
                            <img class="img-fluid" v-bind:src="`/img/${guitar.PhotoUrl}.jpg`" v-bind:alt="`${guitar.Name ? guitar.Name : 'Default guitar'}`">
                        </td>
                        <td>
                            {{ guitar.Name }}
                        </td>
                        <td class="fw-bold">
                            {{ guitar.Price }}
                        </td>
                        <td class="flex align-items-start gap-4">
                            <button type="button" class="btn btn-dark" v-on:click="$emit('decrement-one-guitar', guitar.Id)">-</button>
                            {{ guitar.quantity }}
                            <button type="button" class="btn btn-dark" v-on:click="$emit('increment-one-guitar', guitar.Id)">+</button>
                        </td>
                        <td>
                            <button class="btn btn-danger" type="button" v-on:click="$emit('remove-guitar', guitar.Id)">Remove</button>
                        </td>
                    </tr><!-- End v-for -->
                </tbody>
            </table>

            <p class="text-end">Total pagar: <span class="fw-bold">{{ calculateTotalCartPrice }}</span></p>
            <button class="btn btn-dark w-100 mt-3 p-2" v-on:click="$emit('empty-cart')">Vaciar Carrito</button>
            </div>
        </div>
    </div>
</template>