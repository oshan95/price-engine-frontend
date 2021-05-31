<template>
  <br />
  <form @submit="onSubmit" class="eval-form">
    <div class="dropdown">
      <button
        class="btn btn-secondary dropdown-toggle"
        type="button"
        id="dropdownMenu2"
        data-toggle="dropdown"
        aria-haspopup="true"
        aria-expanded="false"
      >
        {{ productName }}
      </button>
      <div class="dropdown-menu" aria-labelledby="dropdownMenu2">
        <button
          @click="selectProduct(product.product_id, product.product_name)"
          class="dropdown-item"
          type="button"
          v-for="product of products"
          :key="product.product_id"
        >
          {{ product.product_name }}
        </button>
      </div>
    </div>
    <p class="qty-type">Quantity type:</p>
    <div class="form-control-check">
      <div class="form-check">
        <input
          v-model="quantityType"
          class="form-check-input"
          type="radio"
          name="exampleRadios"
          id="exampleRadios1"
          value="units"
        />
        <label class="form-check-label" for="exampleRadios1"> Units </label>
      </div>
      <div class="form-check">
        <input
          v-model="quantityType"
          class="form-check-input"
          type="radio"
          name="exampleRadios"
          id="exampleRadios2"
          value="cartons"
        />
        <label class="form-check-label" for="exampleRadios2"> Cartons </label>
      </div>
    </div>
    <div class="form-group">
      <label for="exampleInputEmail1">Quantity</label>
      <input
        v-model="quantity"
        type="number"
        class="form-control"
        id="exampleInputEmail1"
        aria-describedby="emailHelp"
        placeholder="Enter Quantity"
      />
    </div>
    <button type="submit" class="btn btn-primary">Generate Amount</button>

    <br />
    <div v-if="amount" class="form-group row form-group-result">
      <label for="amount" class="col-sm-2 col-form-label">Net Cost</label>
      <div class="col-sm-10">
        <p id="amount">{{ amount }}</p>
      </div>
    </div>
  </form>
</template>

<script>
import axios from "axios";
export default {
  name: "Calculator",
  data() {
    return {
      quantity: "",
      amount: "",
      quantityType: "",
      products: [],
      productId: "",
      productName: "Select Product",
      form: {
        product_id: "",
        carton_order: false,
        units: "",
      },
    };
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
    onSubmit(e) {
      e.preventDefault();
      if (!this.productId || !this.productName) {
        alert("Please select a product from the dropdown!");
        return;
      } else if (!this.quantity) {
        alert("Please enter a quantity!");
        return;
      }

      this.form.product_id = this.productId;
      this.form.carton_order = this.quantityType === "cartons";
      this.form.units = this.quantity;

      axios
        .post(
          "http://localhost:8509/priceengine/calculator/generate",
          this.form
        )
        .then((response) => {
          this.amount = response.data;
        });
    },
    selectProduct(id, name) {
      this.productName = name;
      this.productId = id;
    },
  },
};
</script>

<style scoped>
.eval-form {
  margin-left: 40px;
}
.form-control-check {
  margin-left: 40px;
}
.form-group-result {
  margin-top: 40px;
}
.qty-type {
  margin-top: 20px;
}
</style>