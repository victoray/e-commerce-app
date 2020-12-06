<template>
  <div class="order-listing-page">
    <h3>Order Listing</h3>

    <b-row align-h="center">
      <b-spinner v-if="loading" variant="primary" type="grow" label="Spinning">
      </b-spinner>
    </b-row>

    <table class="table table-striped" v-if="orders">
      <thead>
        <tr>
          <th scope="col">Order ID</th>
          <th scope="col">Items</th>
          <th scope="col"></th>
        </tr>
      </thead>
      <tbody v-if="orders.length">
        <tr v-for="order in orders" :key="order.id">
          <td>{{ order.id }}</td>
          <td>{{ order.items.length }} item(s)</td>
          <td class="text-right">
            <b-button variant="outline-info" @click="openOrderDetails(order.id)"
              >Details</b-button
            >
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "OrderListingPage",
  components: {},
  data() {
    return {
      orders: null,
      loading: true
    };
  },
  mounted() {
    axios
      .get("https://euas.person.ee/user/orders/")
      .then(response => {
        this.orders = response.data;
      })
      .finally(() => {
        this.loading = false;
      });
  },
  methods: {
    openOrderDetails: function(orderId) {
      this.$router.push("/profile/orders/" + orderId);
    }
  }
};
</script>

<style scoped></style>
