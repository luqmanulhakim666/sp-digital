<template>
  <div class="cart pa-4">
    <div>
      <h2 class="h2--xlarge mb-6">Cart</h2>
      <v-card
        height="80vh"
        flat
        class="cart-items overflow-auto v-responsive pb-4 transparent"
      >
        <template v-if="cartItems.length">
          <div v-for="(item, index) in cartItems" :key="index">
            <card-cart-menu
              class="mb-4"
              :id="item.id"
              :name="item.name"
              :price="item.price"
              :discountPrice="item.discountPrice"
              :image="item.image"
              :toppings="item.toppings"
              @on:removeItem="onRemoveItem(index)"
            />
          </div>
        </template>

        <div
          class="text-center d-flex align-center justify-center flex-column mt-10"
          v-if="!cartItems.length"
        >
          <v-icon size="58" color="gray lighten-5">mdi-cart</v-icon>
          <h5 class="h5--small mt-2 gray--text text--lighten-5 m">
            Your Cart Is Empty
          </h5>
        </div>
      </v-card>
    </div>

    <div class="cart_order-btn">
      <v-btn
        large
        block
        class="primary"
        :disabled="!cartItems.length"
        :loading="state.isLoading"
        @click="onPlaceOrder()"
        >Place Order (${{ this.decimal(grandTotal) }})</v-btn
      >
    </div>

    <v-snackbar v-model="state.showSnackBar" top color="success">
      <div class="d-flex align-center">
        <v-icon>mdi-check-circle</v-icon>
        <h6 class="h6--xsmall ml-2">Order Successfully Placed!</h6>
      </div>
    </v-snackbar>
  </div>
</template>

<script>
import pipe from "@/mixins/pipe";
export default {
  mixins: [pipe],
  data: () => ({
    state: {
      isLoading: false,
      showSnackBar: false,
    },
  }),
  computed: {
    grandTotal() {
      const mapPrice = this.cartItems?.map((x) => {
        return x.discountPrice ? x.discountPrice : x.price;
      });

      const mapToppingsPrice = this.cartItems?.map((x) => {
        return x.toppings?.map((y) => {
          return y.price;
        });
      });

      const mergeToppingPrice = [].concat.apply([], mapToppingsPrice);

      const price = mapPrice?.reduce((prev, curr) => prev + curr, 0);
      const toppingPrice = mergeToppingPrice?.reduce(
        (prev, curr) => prev + curr,
        0
      );

      return price + toppingPrice;
    },

    cartItems() {
      return this.$store.getters["cart"];
    },
  },

  methods: {
    onRemoveItem(index) {
      this.$store.dispatch("onREMOVE_ITEM", index);
    },
    onPlaceOrder() {
      this.state.isLoading = true;
      setTimeout(() => {
        this.state.showSnackBar = true;
        this.state.isLoading = false;
        this.$store.dispatch("onReset");
      }, 500);
    },
  },
};
</script>

<style lang="scss" scoped>
.cart {
  @media (min-width: 600px) {
    height: calc(100vh - 64px);
  }
  position: relative;
  &_order-btn {
    width: 100%;
    left: 50%;
    padding: 0 16px;
    transform: translate(-50%, -50%);
    position: absolute;
    bottom: 0;
  }
}
</style>
