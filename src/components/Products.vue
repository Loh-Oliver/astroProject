<template>
  <div>
    <div class="title-and-filter">
      <div class="header_text">Products</div>
      <div class="filter-dropdown">
        <select
          class="product_sort_container"
          v-model="selectedFilter"
          @change="filterProducts"
        >
          <option value="alphabeticalAZ">Name (A to Z)</option>
          <option value="alphabeticalZA">Name (Z to A)</option>
          <option value="low">Price (Low to High)</option>
          <option value="high">Price (High to Low)</option>
        
        </select>
      </div>
    </div>

    <div class="inventory_container">
      <div class="inventory_list">
        <div
          v-for="(product, index) in products"
          :key="index"
          class="inventory_item"
        >
          <div class="inventory_item_img">
            <img :src="product.image" />
          </div>
          <div class="inventory_item_description">
            <div class="inventory_item_label">
              <astro>
                <a :href="`/items/${product.id}`" class="inventory_item_name">
                  {{ product.name }}
                </a>
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
  data() {
    return {
      products: [],
      selectedFilter: "alphabeticalAZ", // Set the initial value to "alphabeticalAZ"
      isPrimary: true, //Track button style
    };
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    navigateToShop() {
      window.location.href = "/item";
    },
    fetchProducts() {
      fetch("./src/components/products.json")
        .then((response) => response.json())
        .then((data) => {
          this.products = data.products.map((product) => ({
            ...product,
            buttonText: "Add to cart", // Initialize buttonText property
          }));
        })
        .catch((error) => {
          console.error("Error fetching products:", error);
        });
    },
    filterProducts() {
      if (this.selectedFilter === "low") {
        // Sort products by price (low to high)
        this.products.sort(
          (a, b) => parseFloat(a.cost.slice(1)) - parseFloat(b.cost.slice(1))
        );
      } else if (this.selectedFilter === "high") {
        // Sort products by price (high to low)
        this.products.sort(
          (a, b) => parseFloat(b.cost.slice(1)) - parseFloat(a.cost.slice(1))
        );
      } else if (this.selectedFilter === "alphabeticalAZ") {
        // Sort products alphabetically by name (A to Z)
        this.products.sort((a, b) => a.name.localeCompare(b.name));
      } else if (this.selectedFilter === "alphabeticalZA") {
        // Sort products alphabetically by name (Z to A)
        this.products.sort((a, b) => b.name.localeCompare(a.name));
      }
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

      // Update the JSON file
      const updatedProducts = { products: this.products };
      fetch("./src/components/products.json", {
        method: "PUT", // Use PUT method to update the file
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(updatedProducts),
      })
        .then((response) => {
          if (response.ok) {
            console.log("Product cart status updated successfully.");
          } else {
            console.error("Failed to update product cart status.");
          }
        })
        .catch((error) => {
          console.error("Error updating product cart status:", error);
        });
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
  border-top: 1px solid #ededed; /* Example color */
  border-bottom: 1px solid #ededed; /* Example color */
  height: 56px;
  margin-bottom: 50px;
  padding-left: 15px;
  padding-right: 15px;
}

.header_text {
  color: #132322;
  font-family: "DM Mono";
  font-size: 18px;
  font-weight: 500;
  line-height: 48px;
}

.product_sort_container {
  background-image: url("/src/assets/Filtericon.webp");
  background-repeat: no-repeat;
  background-size: 10%;
  background-position: left;
  border: 1px solid #ededed;
  box-sizing: border-box;
  border-radius: 5px;
  padding: 0;
  width: 250px;
  height: 35px;
  margin-right: 30px;
  text-indent: 60px; /* Adjust this value based on the width of the background image */
}

.inventory_container {
  display: block;
  unicode-bidi: isolate;
}

.inventory_list {
  max-width: 1060px;
  margin: 0 auto;
  display: -webkit-flex;
  display: flex;
  -webkit-justify-content: space-between;
  justify-content: space-between;
  -webkit-flex-flow: row wrap;
  flex-flow: row wrap;
  padding: 0 15px 30px;
}

.inventory_item {
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
}

.inventory_item_name {
  display: flex;
  margin-bottom: 20px;
}

.image-container {
  flex: 0 0 auto; /* Don't allow image to grow */
  margin-right: 20px; /* Add space between image and details */
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
  justify-content: space-between;
}
.inventory_item_name {
  font-family: "DM Mono", monospace;
  font-size: 20px;
  font-weight: 500;
  color: #18583a;
  text-decoration: none; /* Remove underline */
}
.inventory_item_name:hover {
  color: #70cc84; /* Change text color on hover */
}

.inventory_item_desc {
  font-size: 16px;
  line-height: 20px;
  color: #132322;
  display: flex;
}

.pricebar {
  display: flex;
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
    flex: 0 0 auto; /* Don't allow image to grow */
    margin-right: 20px; /* Add space between image and details */
  }

  .inventory_item_img img {
    width: 50%;
    display: block; /* Ensures the image is centered */
    margin: 0 auto; /* Centers the image horizontally */
  }

  .pricebar {
    flex-direction: column; /* Change flex-direction to column */
    gap: 20px;
  }

  .btn {
    width: 100%;
  }

  .inventory_item_price {
    width: 100%;
    display: flex;
    justify-content: flex-start;
  }

  .product_sort_container {
    background-image: url("/src/assets/Filtericon.webp");
    background-repeat: no-repeat;
    background-size: 60%;
    background-position: center; /* Adjust the position of the image */
    border: 1px solid #ededed;
    box-sizing: border-box;
    border-radius: 5px;
    padding: 0; /* Remove padding to fit only the image */
    width: 40px; /* Set width to the width of the image */
    height: 35px;
    margin-right: 30px;
    color: transparent;
  }
}
</style>
