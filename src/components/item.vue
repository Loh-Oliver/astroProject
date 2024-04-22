<template>
  <div>
    <div class="title-and-filter">
      <div class="header_text" @click="navigateToShop">Back to products</div>
    </div>

    <div class="inventory_container">
      <div class="inventory_list">
        <div
          v-for="(product, index) in products"
          :key="index"
          class="inventory_item"
        >
          <div class="inventory_item_img">
            <img :src="'../' + product.image" />
          </div>
          <div class="inventory_item_description">
            <div class="inventory_item_label">
              <astro>
                <div>
                  {{ product.name }}
                </div>
              </astro>
              <div class="inventory_item_desc">{{ product.description }}</div>
            </div>
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
export default {
  props: {
    itemId: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      products: [],
      isPrimary: true, //Track button style
    };
  },
  mounted() {
    this.fetchProducts(this.itemId);
  },
  methods: {
    navigateToShop() {
      window.location.href = "/shop";
  },
    fetchProducts(itemId) {
      fetch("../src/components/products.json")
        .then((response) => response.json())
        .then((data) => {
          const product = data.products.find(
            (product) => product.id === parseInt(itemId)
          );

          if (product) {
  
            this.products = [{ ...product, buttonText: "Add to cart" }];
          } else {
            console.error("Product with itemId 1 not found");
          }
        })
        .catch((error) => {
          console.error("Error fetching products:", error);
        });
    },
    addItemToCart(product) {
      if (!product.addedToCart) {
        product.buttonText = "Remove";
        this.$emit("addItemToCart", product);
      } else {
        product.buttonText = "Add to Cart";
        this.$emit("removeItemFromCart", product);
      }
      product.addedToCart = !product.addedToCart;
    },
    toggleCartStatus(productIndex) {
      // Toggle the addedToCart property of the product at the specified index
      this.products[productIndex].addedToCart =
        !this.products[productIndex].addedToCart;
    },
  },
};
</script>

<style>
.title-and-filter {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  font-family: "DM Mono";
  border-top: 1px solid #ededed; 
  border-bottom: 1px solid #ededed; 
  height: 56px;
  margin-bottom: 50px;
  padding-left: 15px;
  padding-right: 15px;
}

.header_text {
  color: #18583a;
  font-family: "DM Mono";
  font-size: 18px;
  font-weight: 500;
  line-height: 48px;
  cursor: pointer;
  margin: 30px;
}

.header_text:hover{
  color:#70cc84;
}



.inventory_list {
  justify-content: center;
  align-items: center;
}

.inventory_item {
  border: 1px solid #ededed;
  background: #fff;
  padding-left: 10px;
  padding-right: 20px;
  display: flex;
  height: 700px;
  box-sizing: border-box;
  border-radius: 8px;
  width: calc(
    100% - 200px
  );
  margin-left: 100px;
}

.inventory_item_name {
  display: flex;
  margin-bottom: 20px;
}

.image-container {
  flex: 0 0 auto; 
  margin-right: 20px; 
}

.inventory_item_img img {
  height: 100%;
  border-bottom-left-radius: 4px;
  border-top-left-radius: 4px;
}

.inventory_item_description {
  flex: 2 1;
  flex-direction: column;
  padding: 20px 34px 20px 20px;
  display: flex;

}
.inventory_item_name {
  font-family: "DM Mono", monospace;
  font-size: 20px;
  font-weight: 500;
  color: #18583a;
  text-decoration: none; 
}
.inventory_item_name:hover {
  color: #70cc84;
}

.inventory_item_desc {
  font-size: 16px;
  line-height: 20px;
  color: #132322;
  display: flex;
  margin-bottom: 50px;
}

.pricebar {
  display:flex;
  justify-content: space-between;
  align-items: flex-end;
}

.inventory_item_price {
  border-top: 1px solid #ededed;
  color: #132322;
  font-size: 20px;
  font-weight: 500;
  line-height: 36px;
  display: inline-block;
  padding-top: 5px;
}

.btn {
  border: none;
  box-sizing: border-box;
  cursor: pointer;
  padding: 12px 30px;
  font-weight: 500;
  font-size: 18px;
  height: 40px;
  width: 200px;
  text-decoration: none;
  border-radius: 4px;
  line-height: 1px;
}

.btn_secondary {
  border: 1px solid #e2231a;
  background-color: #fff;
  color: #e2231a;
}

.btn_primary {
  border: 1px solid #132322;
  color: #132322;
  background-color: #fff;
}

@media screen and (max-width: 1060px) {
  .inventory_item {
    border: 1px solid #ededed;
    background: #fff;
    position: relative;
    display: -webkit-flex;
    display: flex;
    box-sizing: border-box;
    border-radius: 8px;
    height: 240px;
    width: 100%;
    margin-bottom: 12px;
  }
}

@media screen and (max-width: 550px) {
  .inventory_item {
    border: 1px solid #ededed;
    background: #fff;
    position: relative;
    margin-bottom: 12px;
    border-radius: 8px;
    width: 100%;
    height: 100%;
    flex-direction: column;
  }

  .image-container {
    flex: 0 0 auto;
    margin-right: 20px;
  }

  .inventory_item_img img {
    width: 50%;
    display: block; 
    margin: 0 auto; 
  }

  .pricebar {
    flex-direction: column; 
  }

  .btn {
    width: 100%;
  }

  .inventory_item_price {
    width: 100%;
    display: flex;
    justify-content: flex-start;
  }

}
</style>
