<template>
  <!-- Page -->
  <div class="product-page_container">
    <!-- Navigation Bar -->
    <div class="navbar" :class="{ 'shadow': hasScrolled }">
      <div class="navbar-container">
        <div class="navbar-left">
          <!-- Brand Logo -->
          <img class="navbar-left_logo" src="../src/assets/logo.png">
        </div>

        <div class="navbar-right">
          <!-- Wishlist Button -->
          <img class="navbar-right_wishlist" src="../src/assets/heart-empty.png" @click="showModal = true">
          <!-- <img class="navbar-right_cart" src="../src/assets/shopping-cart.png"> -->
        </div>
      </div>
    </div>
    <div class="product-list_container">
      <!-- Header -->
      <div class="product-list_header">
        <div class="product-header_title">
          <h1>Clothes</h1>
        </div>
      </div>
      <!-- Product Actions -->
      <div class="product-list_filter">
        <div class="product-filter_container">
          <input type="text" v-model="searchQuery" placeholder="Search" class="product-filter_search">
          <select v-model="sorting" @change="sortingActions" class="product-filter_sorting">
            <option value="Sort" selected>Sort</option>
            <option value="nameAsc">Alphabetically, A-Z</option>
            <option value="nameDesc">Alphabetically, Z-A</option>
            <option value="priceAsc">Price, low to high</option>
            <option value="priceDesc">Price, high to low</option>
          </select>
        </div>
      </div>
      <div class="product-list_category">
        <div class="product-category_container">
          <div v-for="category in categories" :key="category" class="product-category_items"
            :class="{ 'selected-category': category === selectedCategory }" @click="filterCategory(category)">
            {{ category }}
          </div>
        </div>
      </div>
      <div class="product-list_items">
        <div class="product-list_grid" v-if="filteredProducts.length != 0">
          <list-item v-for="product in filteredProducts" :key="product.id" :product="product"
            @wishlist-toggle="toggleWishlist"></list-item>
        </div>
        <div v-else> No items found.</div>
      </div>
    </div>
    <wishlistModal :showModal="showModal" @close="showModal = false" :products="wishedProducts"></wishlistModal>
  </div>
</template>

<script>
import ListItem from './components/ListItem.vue'
import WishlistModal from './components/WishlistModal.vue'

export default {
  name: 'App',
  components: {
    ListItem,
    WishlistModal
  },
  data() {
    return {
      products: [
        { id: 1, url: 'product1.png', name: 'Contrast Lace Tie Neck Puff Sleeve Crop Tee', category: 'Tops', price: 31.20, wished: false },
        { id: 2, url: 'product2.png', name: 'New Chinese Style Printed Dress With Button Fastening', category: 'Dresses', price: 50.40, wished: false },
        { id: 3, url: 'product3.png', name: 'Solid Color Drawstring Front Shorts', category: 'Bottoms', price: 47.00, wished: false },
        { id: 4, url: 'product4.png', name: 'Contrast Binding Keyhole Neck Crop Tee', category: 'Tops', price: 20.70, wished: false },
        { id: 5, url: 'product5.png', name: 'Solid Split Hem PU Skirt', category: 'Bottoms', price: 34.00, wished: false },
        { id: 6, url: 'product6.png', name: 'Collar Leather Jacket', category: 'Jackets', price: 90.00, wished: false },
        { id: 7, url: 'product7.png', name: 'Lace Up Side Pleated Skirt', category: 'Bottoms', price: 38.70, wished: false },
        { id: 8, url: 'product8.png', name: 'Plaid Pattern Lapel Neck Vest Jacket', category: 'Jackets', price: 58.00, wished: false },
      ],
      searchQuery: '',
      categories: ['All', 'Tops', 'Bottoms', 'Dresses', 'Jackets'],
      selectedCategory: 'All',
      sorting: 'Sort',
      sortKey: 'name',
      sortDirection: 'asc',
      showModal: false,
      hasScrolled: false
    };
  },
  mounted() {
    window.addEventListener('scroll', this.handleScroll);
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.handleScroll);
  },
  computed: {
    filteredProducts() {
      return this.products
        .filter(product =>
          (this.selectedCategory === 'All' || product.category === this.selectedCategory) &&
          product.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        )
        .sort((a, b) => {
          let modifier = this.sortDirection === 'asc' ? 1 : -1;
          return a[this.sortKey] < b[this.sortKey] ? -1 * modifier : 1 * modifier;
        });
    },
    wishedProducts() {
      return this.products.filter(product => product.wished);
    },
  },
  methods: {
    handleScroll() {
      this.hasScrolled = window.scrollY > 0;
    },
    toggleWishlist(productId) {
      const product = this.products.find(p => p.id === productId);
      if (product) product.wished = !product.wished;
    },
    sortingActions() {
      switch (this.sorting) {
        case 'priceAsc':
          this.sort('price', 'asc');
          break;
        case 'priceDesc':
          this.sort('price', 'desc');
          break;
        case 'nameAsc':
          this.sort('name', 'asc');
          break;
        case 'nameDesc':
          this.sort('name', 'desc');
          break;
        default:
          // Handle the case where the placeholder is selected
          break;
      }
    },
    sort(key, direction) {
      this.sortKey = key;
      this.sortDirection = direction;
    },
    filterCategory(category) {
      this.selectedCategory = category;
    },
  }
}
</script>

<style>
@import url('./assets/scss/style.scss');
</style>