<template>
  <div
    class="menu_card white elevation-2 px-2 pt-2 pb-4 rounded-lg pointer fill-height"
    @click="onEmitSelectItem()"
  >
    <img class="menu_card-img rounded-lg full-width" :src="image" alt="menu" />
    <h6 class="text-center h6--xsmall gray--text text--lighten-1">
      {{ name }}
    </h6>
    <div class="menu_card-price d-flex justify-center">
      <p
        v-if="discountPrice"
        class="h7--xxsmall text-decoration-line-through mr-1 gray--text text--lighten-5"
      >
        ${{ this.decimal(discountPrice) }}
      </p>
      <p class="h7--xxsmall secondary--text">${{ this.decimal(price) }}</p>
    </div>
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
  },

  methods: {
    onEmitSelectItem() {
      this.$emit("on:selectItem");
    },
  },
};
</script>

<style lang="scss" scoped>
.menu_card {
  width: 100%;
  &-img {
    max-height: 300px;
  }
}
</style>
