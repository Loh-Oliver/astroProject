<template>
  <div>
    <div class="header_text2">Your Cart</div>

    <div class="list_container">
      <div class="list_desc">
        <div class="cart_quantity_label">QTY</div>
        <div class="cart_desc_label">Description</div>
      </div>
      <div class="cart_list">
        <div class="cart_item" v-for="(product, index) in cart" :key="index">
          <div class="quantity_bought">1</div>
          <div class="inventory_item_description">
            <a href="destination_url_here" class="inventory_item_name">{{
              product.name
            }}</a>
            <div class="inventory_item_desc">{{ product.description }}</div>
            <div class="pricebar">
              <div class="inventory_item_price">{{ product.cost }}</div>
              <button
                :class="[
                  product.addedToCart ? 'btn_secondary' : 'btn_primary',
                  'btn',
                  'btn_inventory',
                ]"
                @click="addItemToCart(product)"
              >
                {{ product.buttonText }}
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Products from "./Products.vue";
export default {
  props: ["cart"],
  methods: {
    removeItemFromCart(product) {
      this.$emit("removeItemFromCart", product);
    },
  },
};
</script>

<style>
.header_text2 {
  color: #132322;
  font-family: "DM Mono", sans-serif;
  font-size: 18px;
  font-weight: 500;
  line-height: 48px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  font-family:
    DM Mono,
    "sans-serif";
  border-top: 1px solid #ededed; /* Example color */
  border-bottom: 1px solid #ededed; /* Example color */
  height: 56px;
  margin-bottom: 20px;
  padding-left: 15px;
  padding-right: 15px;
}

.list_desc {
  display: flex;
  gap: 20px;
  margin-left: 10px;
  margin-right: 10px;
  padding-bottom: 20px;
}

.cart_list {
  border: 1px solid #ededed;
  background: #fff;
  position: relative;
  margin-bottom: 12px;
  display: -webkit-flex;
  display: flex;
  box-sizing: border-box;
  border-radius: 8px;
  height: 240px;
  width: 100%;
  margin-left: 10px;
  margin-right: 10px;
}
.cart_item {
  display: flex;
}
.quantity_bought {
  border: 1px solid #ededed;
  width: 40px;
  height: 40px;
  font: 14px;
  display: flex;
  justify-content: center; /* Center horizontally */
  align-items: center; /* Center vertically */
  margin-left: 10px;
  margin-top: 10px;
  
}
</style>
