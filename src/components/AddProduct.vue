<script setup>
import InputBox from './InputBox.vue'
import CheckBox from './CheckBox.vue'
import SelectionBox from './SelectionBox.vue'
// import { categories } from '../data/categories'
// import { suppliers } from '../data/suppliers'
// import { products } from '../data/products'
</script>
<template>
  <div class="text-center mt-4">
    <h1>Product {{ ProductID ? 'Update ' : 'Add ' }} Form</h1>
  </div>
  <div class="border border-secondary-subtle p-2 rounded bg-body-tertiary">
    <div class="py-2 d-flex">
      <InputBox
        name="ProductID"
        :change="change"
        placeholder="Product ID"
        :value="product[0]?.ProductID"
      />
      <InputBox
        name="ProductName"
        :change="change"
        placeholder="Product Name"
        :value="product[0]?.ProductName"
      />
    </div>
    <div class="py-2 d-flex">
      <SelectionBox
        name="CategoryID"
        :change="change"
        placeholder="Category Name"
        :options="categories"
        identifier="CategoryName"
        :value="product[0]?.CategoryID"
      />
      <SelectionBox
        name="SupplierID"
        :change="change"
        placeholder="Supplier Name"
        :options="suppliers"
        identifier="CompanyName"
        :value="product[0]?.SupplierID"
      />
    </div>
    <div class="py-2 d-flex">
      <InputBox
        name="ReOrderLevel"
        :change="change"
        placeholder="Re-Order Level"
        :value="product[0]?.ReOrderLevel"
      />
      <InputBox
        name="QuantityPerUnit"
        :change="change"
        placeholder="Quantity Per Unit"
        :value="product[0]?.QuantityPerUnit"
      />
    </div>
    <div class="py-2 d-flex">
      <InputBox
        name="UnitPrice"
        :change="change"
        placeholder="Unit Price"
        :value="product[0]?.UnitPrice"
      />
      <InputBox
        name="UnitStock"
        :change="change"
        placeholder="Unit Stock"
        :value="product[0]?.UnitStock"
      />
    </div>
    <div class="py-2 d-flex">
      <InputBox
        name="UnitInOrder"
        :change="change"
        placeholder="Unit In Order"
        :value="product[0]?.UnitInOrder"
      />
      <CheckBox
        name="Discontinued"
        :change="change"
        placeholder="Discontinued"
        :value="product[0]?.Discontinued"
      />
    </div>
  </div>
  <div class="text-center">
    <button class="btn btn-primary btn-sm my-2 mx-2">Clear</button>
    <button class="btn btn-primary btn-sm my-2 mx-2" @click="AddOrUpdate">
      {{ ProductID ? 'Update ' : 'Add ' }}Product
    </button>
  </div>
</template>

<script>
import axios from 'axios'
import {
  ADD_PRODUCT,
  BASE_URL,
  EDIT_PRODUCT,
  GET_CATEGORIES,
  GET_SUPPLIERS,
  SEARCH_PRODUCT
} from '../utils/constants'
export default {
  name: 'AddProduct',
  props: ['search'],
  data() {
    return {
      formData: {
        ProductID: '',
        ProductName: '',
        CategoryID: '',
        SupplierID: '',
        ReOrderLevel: '',
        QuantityPerUnit: '',
        UnitPrice: '',
        UnitStock: '',
        UnitInOrder: '',
        Discontinued: false
      },
      categories: [],
      suppliers: [],
      products: [],
      ProductID: 0,
      product: {}
    }
  },

  created() {
    this.ProductID = this.$route.params.ProductID
    this.getCategories()
    this.getSuppliers()
    if (this.$route.params.ProductID) {
      this.getProducts()
      this.product = {
        ...this.products.filter((product) => {
          return product.ProductID == this.ProductID
        })
      }
    }
  },
  methods: {
    change(e) {
      this.formData = {
        ...this.formData,
        [e.target.name]: e.target.type == 'checkbox' ? e.target.checked : e.target.value
      }
    },
    AddOrUpdate() {
      const endPoint = BASE_URL + (this.ProductID ? EDIT_PRODUCT : ADD_PRODUCT)
      axios
        .post(endPoint, this.formData)
        .then((res) => {
          alert('Success')
          console.log(res)
        })
        .then(() => {
          this.$router.push({
            name: '/'
          })
        })
        .catch((err) => {
          alert('Failed')
          console.log(err)
        })
    },
    getCategories() {
      const endPoint = BASE_URL + GET_CATEGORIES
      axios
        .post(endPoint, this.formData)
        .then((res) => {
          this.categories = res.data
        })
        .catch((err) => {
          console.log(err)
        })
    },
    getSuppliers() {
      const endPoint = BASE_URL + GET_SUPPLIERS
      axios
        .post(endPoint)
        .then((res) => {
          this.suppliers = res.data
        })
        .catch((err) => {
          console.log(err)
        })
    },
    getProducts() {
      const endPoint = BASE_URL + SEARCH_PRODUCT
      axios
        .post(endPoint, {})
        .then((res) => {
          this.products = res.data
        })
        .catch((err) => {
          console.log(err)
        })
    }
  }
}
</script>

<style></style>
