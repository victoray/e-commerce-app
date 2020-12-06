<template>
  <div class="navigation">
    <div>
      <b-navbar toggleable="lg" class="header navbar-dark">
        <b-navbar-brand to="/">E-commerce App</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item to="/cart">Shopping Cart </b-nav-item>
          </b-navbar-nav>

          <b-navbar-nav>
            <b-nav-item to="/help">Help</b-nav-item>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-nav-form>
              <vue-bootstrap-typeahead
                class="search-field"
                :data="products"
                :serializer="i => i.title"
                placeholder="Search for products..."
                @hit="selectSearchResult"
              >
                <template slot="suggestion" slot-scope="{ data, htmlText }">
                  <img :src="data.image" class="search-result-image" />
                  &nbsp;
                  <span v-html="htmlText"></span>
                </template>
              </vue-bootstrap-typeahead>
            </b-nav-form>

            <b-nav-item-dropdown right>
              <!-- Using 'button-content' slot -->
              <template #button-content>
                Profile
              </template>
              <b-dropdown-item to="/profile/orders">My Orders</b-dropdown-item>
            </b-nav-item-dropdown>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>
  </div>
</template>

<script>
import VueBootstrapTypeahead from "vue-bootstrap-typeahead";
import axios from "axios";

export default {
  name: "Navigation",
  components: {
    VueBootstrapTypeahead
  },
  data() {
    return {
      products: []
    };
  },
  mounted() {
    axios.get("https://euas.person.ee/products").then(response => {
      this.products = response.data;
    });
  },
  updated() {
    console.log("updated");
  },

  methods: {
    selectSearchResult: function(product) {
      this.$router.push("/products/" + product.id);
    }
  }
};
</script>

<style scoped>
.header {
  background-color: #001529;
  color: white !important;
}
.search-field {
  width: 400px;
}
.search-result-image {
  max-width: 100px;
  max-height: 100px;
}
</style>
