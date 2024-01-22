<template>
    <div v-if="showModal" class="modal-overlay" @click="closeModal">
        <div class="modal-content" @click.stop>
            <div class="wishlist-table">
                <div v-if="products.length == 0">Your wishlist is empty. Start adding by liking your favourite items.</div>
                <div v-else class="wishlist-item" v-for="product in products" :key="product.id">
                    <p>{{ product.name }}</p>
                    <p class="wishlist-item_price">RM {{ product.price.toFixed(2) }}</p>
                </div>
                <button class="modal-button" @click="closeModal">Close</button>
            </div>
        </div>
    </div>
</template>
  
<script>
export default {
    props: {
        showModal: {
            type: Boolean,
            default: false,
        },
        products: {
            type: Array,
        },
    },
    methods: {
        closeModal() {
            this.$emit('close');
        }
    }
}
</script>