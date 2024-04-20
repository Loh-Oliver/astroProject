<template>
  
  <div class="shopping_container">
    <header>
     
      <nav>
        <div class="burger_menu_wrapper">
          <div class="burger_menu">
            <!--  <component :is="Slide" v-if="Slide">
              <a id="home" href="#">
                <span>Home</span>
              </a>
            </component> -->
            ≡
          </div>
        </div>
        <div class="header_name">Swag Labs</div>
        <div class="cart_btn">
          <a class="shopping_cart_link" v-on:click="navigateTo('cart')">
            <span v-if="cart.length > 0" class="cart_notification">
              {{ cart.length }}
            </span>
            <!-- Circle span for notification -->
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
      <div class="footer_copy" data-test="footer-copy">
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
    };
  },

  methods: {
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
.burger_menu_wrapper .burger_menu .bm-burger-button {
  /* Your custom styles here */
  position: fixed;
  width: 40px;
  height: 40px;
  left: 10px;
  top: 10px;
  cursor: pointer;
}
.header_name {
  text-align: center;
  font-size: 33px;
  line-height: 48px;
  text-align: center;
  margin: 0 50px;
}

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

.footer {
  background-color: #132322; /* Background color of the footer */
  padding: 20px; /* Padding around the footer content */
  text-align: center; /* Center align the content within the footer */
  color: white;
}

.social {
  list-style: none; /* Remove default list styles */
  padding: 0; /* Remove default padding */
}

.social li {
  display: inline-block; /* Display social icons horizontally */
  margin-right: 10px; /* Add some space between social icons */
}

.social li a {
  display: inline-block; /* Ensure anchor tags behave as blocks */
  width: 40px; /* Set the width and height of the circular icon */
  height: 40px;
  border-radius: 50%; /* Make the icon circular */
  background-color: #70cc84; /* Background color of the icon */
  text-align: center; /* Center align the text */
  line-height: 40px; /* Vertically center the text */
}

.social li a:hover {
  background-color: #aaa; /* Darken the background color on hover */
}

.social_icon {
  width: 20px; /* Adjust the width of the icon */
  height: 20px; /* Adjust the height of the icon */

  overflow: hidden; /* Hide any overflow beyond the circle */
  margin-top: 9px;
}

.footer_copy a {
  color: #333; /* Set link color */
  text-decoration: none; /* Remove underline */
}

.footer_copy a:hover {
  text-decoration: underline; /* Underline on hover */
}
</style>
