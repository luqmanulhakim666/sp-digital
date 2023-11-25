<template>
  <div class="menu_page">
    <v-container fluid class="pa-0">
      <v-row>
        <v-col
          class="menu_page-left"
          cols="12"
          md="9"
          style="overflow-y: auto !important"
        >
          <v-row class="pa-6">
            <v-col
              cols="12"
              sm="6"
              md="4"
              xl="3"
              v-for="(menu, index) in menus"
              :key="index"
            >
              <card-menu
                :id="menu.id"
                :name="menu.name"
                :price="menu.price"
                :discountPrice="menu.discountPrice"
                :image="menu.image"
                @on:selectItem="onSelectItem(menu)"
              />
            </v-col>
          </v-row>
        </v-col>
        <v-col cols="12" md="3" class="pl-0">
          <cart class="gray fill-height" />
        </v-col>
      </v-row>
    </v-container>

    <dialog-toppings
      :dialog="state.dialogToppings"
      @add:toCart="onAddToCart"
      @on:close="handleDialogToppings"
    />
  </div>
</template>

<script>
export default {
  data: () => ({
    state: {
      dialogToppings: false,
      selectedItem: {},
    },
  }),

  computed: {
    menus() {
      return this.$store.getters["menus"];
    },
  },

  methods: {
    onAddToCart(selectedToppings) {
      const payload = {
        ...this.state.selectedItem,
        toppings: selectedToppings,
      };

      this.$store.dispatch("onADD_TO_CART", payload);
      this.state.dialogToppings = false;
    },
    onSelectItem(val) {
      this.state.selectedItem = val;
      this.state.dialogToppings = true;
    },
    handleDialogToppings() {
      this.state.dialogToppings = !this.state.dialogToppings;
      this.state.selectedItem = {};
    },
  },
};
</script>

<style lang="scss" scoped>
.menu_page {
  &-left {
    overflow-y: auto;
    height: 100vh;
    @media (max-width: 900px) {
      height: auto;
    }
  }
}
</style>
