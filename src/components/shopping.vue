<template>
  <div class="shopping_container">
    <header>
      <nav>
        <div>
          <!-- Add a click event handler to toggle the 'active' class -->
          <div
            class="burger_menu"
            @click="toggleMenu"
            :class="{ active: isMenuActive }"
          >
            ≡
          </div>
          <!-- Apply the 'active' class conditionally based on the 'isMenuActive' state -->
          <div class="off_screen_menu" :class="{ active: isMenuActive }">
            <div class="off_screen_btn" @click="toggleMenu">✕</div>
            <span>All Items</span>
            <span>About</span>
            <span>Logout</span>
            <span>Reset App State</span>
          </div>
        </div>
        <div class="header_name">Swag Labs</div>
        <div class="cart_btn">
          <a class="shopping_cart_link" v-on:click="navigateTo('cart')">
            <span v-if="cart.length > 0" class="cart_notification">
              {{ cart.length }}
            </span>
            <img
              src="/src/assets/Shoppingcarticon.png"
              alt="Shopping Cart"
              width="24"
              height="24"
            />
          </a>
        </div>
      </nav>
    </header>

    <div v-if="page === 'cart'">
      <Cart v-on:removeItemFromCart="removeItemFromCart" :cart="cart" />
      <div class="cart_footer">
        <button class="btn product_btn" v-on:click="navigateTo('products')">
          Continue Shopping
        </button>
        <button class="btn checkout_btn" v-on:click="navigateTo('products')">
          Checkout
        </button>
      </div>
    </div>

    <div v-if="page === 'products'">
      <Products
        @addItemToCart="addItemToCart"
        @removeItemFromCart="removeItemFromCart"
      />
    </div>
    <footer class="footer" data-test="footer">
      <ul class="social">
        <li class="social_twitter">
          <a
            href="https://twitter.com/saucelabs"
            target="_blank"
            rel="noreferrer"
            data-test="social-twitter"
          >
            <img
              class="social_icon"
              src="/src/assets/twitter.svg"
              alt="Facebook Icon"
          /></a>
        </li>
        <li class="social_facebook">
          <a
            href="https://www.facebook.com/saucelabs"
            target="_blank"
            rel="noreferrer"
            data-test="social-facebook"
          >
            <img
              class="social_icon"
              src="/src/assets/facebook.png"
              alt="Facebook Icon"
            />
          </a>
        </li>
        <li class="social_linkedin">
          <a
            href="https://www.linkedin.com/company/sauce-labs/"
            target="_blank"
            rel="noreferrer"
            data-test="social-linkedin"
          >
            <img
              class="social_icon"
              src="/src/assets/linkedin.svg"
              alt="Facebook Icon"
          /></a>
        </li>
      </ul>
      <div class="footer_" data-test="footer-copy">
        © 2024 Sauce Labs. All Rights Reserved. Terms of Service | Privacy
        Policy
      </div>
    </footer>
  </div>
</template>

<script>
import Products from "./Products.vue";
import Cart from "./cart.vue";

export default {
  name: "App",
  data: () => {
    return {
      page: "products",
      cart: [],
      isMenuActive: false, // Initially, the menu is not active
    };
  },

  methods: {
    toggleMenu() {
      // Toggle the 'isMenuActive' state
      this.isMenuActive = !this.isMenuActive;
    },
    addItemToCart(product) {
      this.cart.push(product);
    },
    removeItemFromCart(product) {
      this.cart.splice(this.cart.indexOf(product), 1);
    },
    navigateTo(page) {
      this.page = page;
    },
  },

  components: { Products, Cart },
};
</script>

<style>
.shopping_container {
  width: 100vw; /* Set width to 100% of the viewport width */
  height: 100vh; /* Set height to 100% of the viewport height */
  overflow-x: hidden; /* Hide horizontal overflow */
  overflow-y: auto; /* Enable vertical scrolling if needed */
  margin: 0; /* Remove default margin */
  padding: 0; /* Remove default padding */
}
body {
  margin: 0;
}

.products {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.products button {
  padding: 10px;
  background-color: black;
  color: white;
  outline: none;
  border: none;
  cursor: pointer;
}
</style>

<style scoped>
header {
  position: relative;
  height: 60px;
  text-align: right;
  font-size: 30px;
  padding-top: 5px;
}

header button {
  padding: 10px;
  border: none;
  cursor: pointer;
  color: white;
  background-color: green;
}

nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
}
.header_name {
  text-align: center;
  font-size: 33px;
  line-height: 48px;
  margin: 0 50px;
}

/* burger style */
.off_screen_menu {
  background-color: white;
  height: 100vh;
  width: 100%;
  max-width: 300px;
  position: fixed;
  top: 0;
  z-index: 999; /* Ensure the overlay appears in front */
  display: flex;
  flex-direction: column;
  align-items:center;
  text-align: left;
  font-size: 3rem;
  left: -450px;
  transition: 0.3s ease;
}

.off_screen_menu.active {
  left: 0;
}

.off_screen_menu span {
  width:200px;
  border-bottom: 1px solid #ededed;
  color: #18583a;
  display: inline-block;
  font-family: "DM Mono", "sans-serif";
  font-size: 16px;
  margin-bottom: 7px;
  text-decoration: none;
  cursor: pointer;
  padding: 10px 0;
}

.off_screen_btn {
  align-self: flex-end;
  margin-right: 10px;
  align-items: flex-end;
  cursor: pointer;
  margin-bottom: 10px;
}

/* ham menu */
.burger_menu {
  height: 50px;
  width: 40px;
  margin-left: auto;
  position: relative;
  cursor: pointer;
}

/* cart style */

.cart_btn {
  display: relative;
  align-items: center;
  margin-right: 15px;
}

.cart_notification {
  position: absolute; /* Position the circle span relative to its parent */
  top: 15px; /* Adjust the top position */
  right: 5px; /* Adjust the right position */
  background-color: red; /* Background color of the circle */
  color: white; /* Text color */
  width: 25px; /* Width of the circle */
  height: 25px; /* Height of the circle */
  border-radius: 50%; /* Make it a circle */
  font-size: 12px; /* Font size of the number */
  display: flex; /* Use flexbox for centering content */
  justify-content: center; /* Center horizontally */
  align-items: center; /* Center vertically */
}

.product_btn {
  width: calc(100% - 40px);
  margin-left: 15px;
  margin-right: 15px;
  border-radius: 4px;
  background-color: white;
  border: 2px solid black;
  margin-bottom: 20px;
  overflow-x: auto; /* Enable horizontal scrolling if content overflows */
}

.checkout_btn {
  width: calc(100% - 40px);
  margin-left: 15px;
  margin-right: 15px;
  border-radius: 4px;
  background-color: #70cc84;
  border: none;
  margin-bottom: 100%;
}

/* footer style */
.footer {
  background-color: #132322; /* Background color of the footer */
  padding: 20px; /* Padding around the footer content */
  text-align: center; /* Center align the content within the footer */
  color: white;
}

.social li {
  display: inline-block;
  margin-right: 10px;
}

.social li a {
  display: inline-block;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #70cc84;
  text-align: center;
  line-height: 40px;
}

.social li a:hover {
  background-color: green;
}

.social_icon {
  width: 20px; /* Adjust the width of the icon */
  height: 20px; /* Adjust the height of the icon */
  overflow: hidden; /* Hide any overflow beyond the circle */
  margin-top: 9px;
}
</style>
