<template>
  <div class="menu_card white elevation-1 pa-2 d-flex rounded-lg">
    <v-row no-gutters>
      <v-col md="4" xs="2" sm="2" align-self="center">
        <img
          class="menu_card-img rounded-lg fill-height"
          :src="image"
          alt="menu"
        />
      </v-col>

      <v-col md="8" xs="10" sm="10" align-self="center">
        <div class="ml-2 flex-column justify-space-between">
          <h6 class="h6--xsmall gray--text text--lighten-1">
            {{ name }}
          </h6>

          <template v-if="!toppings.length">
            <span class="text--small">No Topping</span>
          </template>

          <template v-if="toppings.length">
            <div class="d-flex mb-4 flex-wrap">
              <span
                class="text--small"
                v-for="(topping, index) in toppings"
                :key="index"
              >
                {{ topping.name
                }}<span v-if="!isLastIndex(index)" class="mr-1">,</span>
              </span>
            </div>
          </template>

          <div class="d-flex align-center justify-space-between">
            <div class="d-flex">
              <p
                v-if="discountPrice"
                class="h7--xxsmall text-decoration-line-through mr-1 gray--text text--lighten-5"
              >
                $ {{ this.decimal(discountPrice) }}
              </p>
              <p class="h7--xxsmall secondary--text">
                $ {{ this.decimal(price) }}
              </p>
            </div>
            <v-btn small icon color="error" @click="onEmitRemoveItem()">
              <v-icon>mdi-delete-outline</v-icon></v-btn
            >
          </div>
        </div>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import pipe from "@/mixins/pipe";
export default {
  mixins: [pipe],
  props: {
    id: String,
    name: String,
    price: Number,
    discountPrice: Number,
    image: String,
    toppings: Array,
  },

  methods: {
    isLastIndex(index) {
      return this.toppings?.length === index + 1;
    },

    onEmitSelectItem() {
      this.$emit("on:selectItem");
    },

    onEmitRemoveItem() {
      this.$emit("on:removeItem");
    },
  },
};
</script>

<style lang="scss" scoped>
.menu_card {
  width: 100%;
  &-img {
    width: 100%;
    max-height: 80px;
  }
}
</style>
