<template>
  <DropDownMenu menu-title="Select Product">
    <section
      @click="displayDynamicTable(product.product_id, product.product_name)"
      class="option"
      v-for="product of products"
      :key="product.product_id"
    >
      <button>
        <h4>{{ capitalizeFirstLetter(product.product_name) }}</h4>
      </button>
    </section>
  </DropDownMenu>

  <table id="tableAvant" class="table mt-5">
    <thead>
      <tr>
        <th scope="col">Product Name</th>
        <th scope="col">Quantity</th>
        <th scope="col">Price</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="entry in rows" v-bind:key="entry.id">
        <td>{{ getProductName() }}</td>
        <td>{{ entry.quantity }}</td>
        <td>{{ entry.price }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import axios from "axios";
import Header from "../components/Header";
import DropDownMenu from "../components/DropdownMenu";
import { createRouter, createWebHistory } from "vue-router";

export default {
  name: "Home",
  data() {
    return {
      products: [],
      rows: [],
      productName: null,
    };
  },
  components: {
    DropDownMenu,
    Header,
  },
  async mounted() {
    try {
      const res = await axios.get("http://localhost:8509/priceengine/products");
      this.products = res.data;
    } catch (e) {
      console.log(e);
    }
  },
  methods: {
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    displayDynamicTable(id, string) {
      this.productName = string;
      axios
        .get(
          "http://localhost:8509/priceengine/calculator/prices?product_id=" + id
        )
        .then((response) => {
          this.rows = response.data;
        });
    },
    getProductName() {
      return this.productName;
    },
  },
};
</script>

<style lang="scss">
.option {
  width: 100%;
  border-bottom: 1px solid #eee;
  padding: 20px 0;
  cursor: pointer;
  position: relative;
  z-index: 2;

  &:last-child {
    border-bottom: 0;
  }

  * {
    color: inherit;
    text-decoration: none;
    background: none;
    border: 0;
    padding: 0;
    outline: none;
    cursor: pointer;
  }
}

.desc {
  opacity: 0.5;
  display: block;
  width: 100%;
  font-size: 14px;
  margin: 3px 0 0 0;
  cursor: default;
}
</style>