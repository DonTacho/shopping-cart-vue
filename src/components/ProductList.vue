<template>
    <div>
        <h1>Product List</h1>
        <img
            v-if="loading"
            src="https://i.imgur.com/JfPpwOA.gif"
        >
        <ul v-else>
            <li v-for="(product, index) in products" :key="index">
                Stock: {{ product.inventory }}, Name: {{ product.title }}, Price: {{ product.price | currency }}
                <button
                :disabled="!productIsInStock(product)"
                @click="addProductToCart(product)">Add to cart</button>
            </li>
        </ul>
    </div>
  
</template>

<script>

    import { mapState, mapGetters, mapActions } from 'vuex'

    export default {
        data () {
            return {
                loading: false
            }
        },


        computed: {
            // ES7 Spread Operator
             ...mapState('products', {
                products: state => state.items
            }),

            ...mapGetters('products', {
                productIsInStock: 'productIsInStock'
            })

        },

        methods: {
            // ES7 Spread Operator
            ...mapActions({
                fetchProducts: 'products/fetchProducts',
                addProductToCart: 'addProductToCart'
            })
        },

        created () {
            this.loading = true
            this.fetchProducts()
            .then(()=> this.loading = false)
        }
    }
</script>

<style>

</style>