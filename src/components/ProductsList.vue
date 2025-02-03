<script>
import {notebooksList} from "@/constants/notebook-data";
import ProductCard from "@/components/ProductCard.vue";
import ProductsFilters
  from "@/components/products-filters/ProductsFilters.vue";

export default {
  name: "ProductsList",
  components: {ProductsFilters, ProductCard},
  data() {
    return {
      notebooksList,
      productSellers: [],
      productBrands: [],
    }
  },
  computed: {
    filteredNotebooksList() {
      if (this.productSellers.length > 0 || this.productBrands.length > 0) {
        return notebooksList.filter(prod => this.isProductAccepted(prod))
      }
      return notebooksList
    },

  },
  methods: {
    isProductAccepted(prod) {
      const seller = this.productSellers.length === 0 ||
          ((this.productSellers.includes('rozetka') && prod.seller === 'rozetka') ||
              (this.productSellers.includes('seller') && prod.seller !== 'rozetka'))
      const brand = this.productBrands.length === 0 ||
          (this.productBrands.includes(prod.brand.toLowerCase()))
      return seller && brand;
    },
  }
}
</script>

<template>
  <div class="wrapper">
    <products-filters
        v-model:sellers.check='productSellers'
        v-model:brands.check='productBrands'
        :notebooksList='notebooksList'
    :filteredNotebooksList = 'filteredNotebooksList'></products-filters>
    <div class="products__container">
      <div class="products__cards">

        <product-card
            v-for="product in filteredNotebooksList"
            :key='product.id'
            :product="product"></product-card>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  display: flex;
  gap: 30px;
}

.products__container {
  padding: 50px 15px;
  margin: 0 auto;

}

.products__cards {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 50px;
}
</style>