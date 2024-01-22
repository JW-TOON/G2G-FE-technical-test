<template>
    <div class="list-item">
        <div class="list-item_image">
            <img :src="getImageUrl(product.url)" alt="Product Image" class="product-image" />
            <div v-if="product.wished" @click="wishToggle">
                <svg class="clickable-image" width="24" height="24" viewBox="0 0 48 48" fill="none"
                    xmlns="http://www.w3.org/2000/svg">
                    <path
                        d="M15 8C8.92487 8 4 12.9249 4 19C4 30 17 40 24 42.3262C31 40 44 30 44 19C44 12.9249 39.0751 8 33 8C29.2797 8 25.9907 9.8469 24 12.6738C22.0093 9.8469 18.7203 8 15 8Z"
                        fill="#003986" stroke="#333" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" />
                </svg>
            </div>
            <div v-else @click="wishToggle">
                <svg class="clickable-image" width="24" height="24" viewBox="0 0 48 48" fill="none">
                    <path
                        d="M15 8C8.92487 8 4 12.9249 4 19C4 30 17 40 24 42.3262C31 40 44 30 44 19C44 12.9249 39.0751 8 33 8C29.2797 8 25.9907 9.8469 24 12.6738C22.0093 9.8469 18.7203 8 15 8Z"
                        fill="none" stroke="#333" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" />
                </svg>
            </div>
        </div>
        <div class="list-item_description">
            <h3>{{ product.name }}</h3>
            <div>RM {{ product.price.toFixed(2) }}</div>
        </div>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            publicPath: process.env.BASE_URL
        }
    },
    props: {
        product: {
            type: Object,
            default: () => { },
        },
    },
    computed: {
    },
    methods: {
        toggleWishlist() {
            this.$emit('wishlist-toggle', this.product.id);
        },
        wishToggle() {
            this.toggleWishlist();
        },
        getImageUrl(imageFileName) {
            return require(`@/assets/${imageFileName}`);
        }
    }
}
</script>