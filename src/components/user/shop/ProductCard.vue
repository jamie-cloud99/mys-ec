<template>
  <router-link
    :to="`/product/${product.id}`"
    class="card border-0 bg-transparent h-100 overflow-hidden"
  >
    <div class="ratio ratio-1x1">
      <img
        :src="product.imageUrl"
        :alt="product.title"
        class="product-img rounded-md"
        loading="lazy"
        width="1000"
        height="1000"
      />
    </div>
    <div class="card-body pt-3 pb-0">
      <h3 class="fs-6 mb-1">{{ product.title }}</h3>
      <div class="d-flex align-items-center mb-2">
        <p class="align-baseline">
          NT$ {{ $format.currency(product.price) }}
          <s v-if="product.price!==product.origin_price" class="text-secondary fs-sm ms-2 align-baseline"
            >NT$ {{ $format.currency(product.origin_price) }}</s
          >
        </p>
      </div>
    </div>
    <div class="card-footer bg-transparent border-0">
      <button
        type="button"
        class="btn btn-outline-primary w-100 rounded-sm px-3"
        @click.prevent="addToCart(product.id)"
        :disabled="cartLoadingItem === product.id"
      >
        加入購物車
        <span
          v-if="cartLoadingItem === product.id"
          class="spinner-border spinner-border-sm"
          role="status"
          aria-hidden="true"
        ></span>
        <span v-if="cartLoadingItem === product.id" class="visually-hidden">Loading...</span>
      </button>
    </div>
  </router-link>
</template>

<script>
import { mapState, mapActions } from 'pinia'
import cartStore from '../../../stores/cartStore'
import statusStore from '../../../stores/statusStore'

export default {
  props: ['product'],
  computed: {
    ...mapState(cartStore, ['cartList']),
    ...mapState(statusStore, ['isLoading', 'cartLoadingItem'])
  },
  methods: {
    ...mapActions(cartStore, ['addToCart'])
  }
}
</script>

<style lang="scss">
.product-img {
  object-fit: cover;
}
</style>
