<script>
export default {
  name: "ProductsFilters",
  props: {
    sellers: {
      type: Array,
      default: () => []
    },
    sellersModifiers: {default: () => ({})},
    brands: {
      type: Array,
      default: () => []
    },
    brandsModifiers: {default: () => ({})},
    notebooksList: {
      type: Array,
      default: () => []
    },
    filteredNotebooksList: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      searchBrand: null,
    }
  },
  computed: {
    checkedSellerValue: {
      get() {
        return this.sellers
      },
      set(val) {
        this.$emit('update:sellers', val)
      },
    },
    checkedBrandValue: {
      get() {
        return this.brands
      },
      set(val) {
        this.$emit('update:brands', val)
      },
    },
    uniqueBrands() {
      if (this.checkedSellerValue.includes('rozetka') && this.checkedSellerValue.includes('seller')) {
        return [...new Set(this.notebooksList.map(el => el.brand))];
      }
      if (this.checkedSellerValue.includes('rozetka')) {
        return [...new Set(this.notebooksList
            .filter(el => el.seller === 'rozetka')
            .map(el => el.brand))];
      }
      if (this.checkedSellerValue.includes('seller')) {
        return [...new Set(this.notebooksList
            .filter(el => el.seller !== 'rozetka')
            .map(el => el.brand))];
      }
      return [...new Set(this.notebooksList.map(el => el.brand))];
    },

    searchedUniqueBrands() {
      if (this.searchBrand) {
        return this.uniqueBrands.filter(brand => this.isBrandAccepted(brand))
      }
      return this.uniqueBrands
    },
    qntyProductsRoz() {
      return this.filteredNotebooksList.filter(el => el.seller === 'rozetka').length
    },
    qntyProductsOther() {
      return this.filteredNotebooksList.filter(el => el.seller !== 'rozetka').length
    },
    brandCounts() {
      return this.searchedUniqueBrands.reduce((acc, brand) => {
        acc[brand] = this.filteredNotebooksList.filter(el => el.brand === brand).length;
        return acc;
      }, {});
    },
    sellerSectionBorder() {
      return this.sellersModifiers.check && this.sellers.length === 0;
    },
    brandSectionBorder() {
      return this.brandsModifiers.check && this.brands.length === 0;
    }
  },
  methods: {
    isBrandAccepted(brand) {
      return brand.toLowerCase().includes((this.searchBrand.toLowerCase()))
    }
  }
}
</script>

<template>
  <div class="container">
    <div class="section" :class="{border: sellerSectionBorder}">
      <h3 class="title">Продавець</h3>
      <div class="filters">
        <input type="checkbox" id="rozetka" value="rozetka"
               v-model="checkedSellerValue"/>
        <label class="label" for="rozetka">Rozetka</label>
        <span>{{ qntyProductsRoz }}</span>
      </div>
      <div class="filters">
        <input type="checkbox" id="seller" value="seller"
               v-model="checkedSellerValue"/>
        <label class="label" for="seller">Інші
          продавці</label>
        <span>{{ qntyProductsOther }}</span>
      </div>
    </div>
    <div class="section">
      <h3 class="title">Бренд</h3>
      <input type='text' placeholder="Пошук" class="input"
             v-model.trim="searchBrand"/>
    </div>
    <div class="section" :class="{border: brandSectionBorder}">
      <h3 class="title">
        Алфавітний
        вказівник</h3>
      <div v-for="(brand, index) in searchedUniqueBrands"
           :key='index' class="filters">
        <input type="checkbox" :id="brand"
               :value="brand"
               v-model="checkedBrandValue"/>
        <label class="label" :for="brand">
          {{
            brand.charAt(0).toUpperCase() + brand.slice(1)
          }}</label>
        <span>{{ brandCounts[brand] }}</span>
      </div>
    </div>
  </div>

</template>

<style scoped>
.container {
  min-width: 300px;
  display: flex;
  flex-direction: column;
  row-gap: 20px;
}

.section {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 10px;
}

.filters {
  display: flex;
  gap: 10px;
}

.label {
  display: flex;
  align-items: center;
  gap: 10px;
}

.title {
  color: dodgerblue;
}

.input {
  height: 30px;
  border-radius: 8px;
  border: 1px solid #3c4242;
  padding: 16px 20px;
  width: 80%;
}

.border {
  border: 1px solid green;
  border-radius: 8px;
}
</style>