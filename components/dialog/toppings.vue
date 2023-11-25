<template>
  <v-dialog v-model="dialog" persistent width="800">
    <div class="white rounded-lg">
      <div class="d-flex justify-space-between pa-4">
        <p>Toppings</p>

        <v-btn small icon @click="onEmitClose()">
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </div>

      <v-divider />

      <div class="pa-4">
        <v-row no-gutters>
          <v-col
            cols="12"
            sm="4"
            v-for="(topping, index) in toppings"
            :key="index"
          >
            <v-checkbox
              v-model="selectedToppings"
              :ripple="false"
              :value="topping"
            >
              <template v-slot:label>
                <span class="text--default mr-1">{{ topping.name }}</span>
                <span class="text--default">(${{ topping.price }})</span>
              </template>
            </v-checkbox>
          </v-col>
        </v-row>
        <div class="d-flex justify-end mt-6">
          <v-btn
            depressed
            class="primary text-capitalize"
            @click="onEmitAddToCart()"
            >Add To Cart</v-btn
          >
        </div>
      </div>
    </div>
  </v-dialog>
</template>

<script>
export default {
  props: {
    dialog: Boolean,
  },

  data: () => ({
    selectedToppings: [],
  }),

  computed: {
    toppings() {
      return this.$store.getters["toppings"];
    },
  },

  methods: {
    onEmitAddToCart() {
      this.$emit("add:toCart", this.selectedToppings);
    },

    onEmitClose() {
      this.$emit("on:close");
    },
  },

  watch: {
    dialog(val) {
      if (!val) {
        this.selectedToppings = [];
      }
    },
  },
};
</script>
