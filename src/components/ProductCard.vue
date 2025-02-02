<script>
export default {
  name: "ProductCard",
  props: {
    product: {
      type: Object,
      required: true
    }
  }
}
</script>

<template>
  <article class="products__card">
    <a class="products__link-img" :href="product.link">
      <img class="products__image" :src="product.imgSrc"
           :alt="product.title">
    </a>
    <div class="products__body">
      <div class="products__content">
        <h3>
          <a :href="product.link" class="products__label">{{ product.title }}</a>
        </h3>
        <span class="products__text">{{product.price}}</span>
      </div>
    </div>
  </article>
</template>

<style scoped>

.products__card {
  display: flex;
  flex-direction: column;
  min-height: 100%;
  row-gap: 16px;
  border-radius: 10px;
}

.products__link-img {
  overflow: hidden;
}

.products__image {
  width: 100%;
  height: 100%;
  aspect-ratio: 1/1;
  object-fit: contain;
}

.products__body {
  flex-grow: 1;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.products__content {
  display: flex;
  flex-direction: column;
  row-gap: 4px;
}

.products__label {
  color: #2a2f2f;
  font-size: 17.875px;
  font-weight: 700;
  line-height: 1.56;
  letter-spacing: -0.715px;
}

.products__text {
  color: red;
  font-size: 18px;
  font-weight: 500;
  line-height: 2.08;
  letter-spacing: -0.536px;
}

</style>