<template>
  <div class="product-panel">
    <h2 v-if="categoryAlias">Listing for category: {{ categoryAlias }}</h2>
    <h2 v-if="!categoryAlias">{{ title }}</h2>

    <b-row align-h="center">
      <b-spinner
        v-if="loading"
        variant="primary"
        type="grow"
        label="Spinning"
      ></b-spinner>
    </b-row>

    <b-card-group deck v-if="products">
      <ProductItem
        v-for="productItem in products"
        :product="productItem"
        :key="productItem.id"
      />
    </b-card-group>
  </div>
</template>

<script>
import ProductItem from "./ProductItem.vue";
import axios from "axios";

export default {
  name: "ProductPanel",
  components: {
    ProductItem
  },
  props: {
    title: String,
    categoryAlias: String,
    section: String
  },
  data() {
    return {
      products: null,
      loading: true
    };
  },
  mounted() {
    if (this.categoryAlias) {
      axios
        .get(
          "https://euas.person.ee/categories/" +
            this.categoryAlias +
            "/products"
        )
        .then(response => {
          this.products = response.data;
        })
        .finally(() => {
          this.loading = false;
        });
    } else if (this.section) {
      axios
        .get("https://euas.person.ee/sections/" + this.section + "/products")
        .then(response => {
          this.products = response.data;
        })
        .finally(() => {
          this.loading = false;
        });
    }
  }
};
</script>

<style scoped></style>
