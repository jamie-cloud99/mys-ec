<template>
  <div class="d-flex justify-content-center align-items-center">
    <button
      class="btn quantity-btn btn-outline-primary rounded-circle flex-shrink-0"
      type="button"
      @click.prevent="updateNum('minus')"
      :disabled="tempQuantity <= 1"
    >
      <i class="bi bi-dash"></i></button
    ><input
      class="form-control quantity text-center border border-primary bg-transparent mx-2"
      type="number"
      min="1"
      v-model.number="tempQuantity"
      disabled
    />
    <button
      class="btn quantity-btn btn-outline-primary rounded-circle flex-shrink-0"
      type="button"
      @click.prevent="updateNum('add')"
      :disabled="tempQuantity >= 10"
    >
      <i class="bi bi-plus"></i>
    </button>
  </div>
</template>

<script>
import {mapState, mapActions} from 'pinia'
import statusStore from '../../../stores/statusStore'
import cartStore from '../../../stores/cartStore'

export default {
  props: {
    quantity: {
      type: Number,
      default: 1
    }
  },
  emits: ['update'],
  data() {
    return {
      tempQuantity: 1
    }
  },
  watch: {
    quantity() {
      this.tempQuantity = this.quantity
    }
  },
  computed: {
    ...mapState(statusStore, ['isLoading']),
    ...mapState(cartStore, ['cartList'])
  },
  methods: {
    updateNum(action) {
      action === 'add'
        ? this.$emit('update', ++this.tempQuantity)
        : this.$emit('update', --this.tempQuantity)
    },
    ...mapActions(cartStore, ['addToCart'])

  },
  created() {
    this.tempQuantity = this.quantity
  }
}
</script>

<style lang="scss">
.quantity-btn {
  width: 1.5rem;
  height: 1.5rem;
  padding: 0;
}

.quantity {
  height: 2rem;
  width: 4rem;
}

.bi::before {
  line-height: 1.5;
  vertical-align: 0.1em;
}
</style>
