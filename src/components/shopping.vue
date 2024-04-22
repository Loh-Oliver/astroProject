<template>
  <div class="shopping_container">
    <header>
      <nav>
        <div>
          <div
            class="burger_menu"
            @click="toggleMenu"
            :class="{ active: isMenuActive }"
          >
            ≡
          </div>
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
        <div class="cart_footer_spacer"></div>
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
import Products from "./products.vue";
import Cart from "./cart.vue";

export default {
  name: "App",
  data: () => {
    return {
      page: "products",
      cart: [],
      isMenuActive: false,
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
  width: 100vw;
  height: 100vh;
  overflow-x: hidden;
  overflow-y: auto;
  margin: 0;
  padding: 0;
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
  align-items: center;
  text-align: left;
  font-size: 3rem;
  left: -450px;
  transition: 0.3s ease;
}

.off_screen_menu.active {
  left: 0;
}

.off_screen_menu span {
  width: 200px;
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


.cart_btn {
  display: relative;
  align-items: center;
  margin-right: 15px;
}

.cart_notification {
  position: absolute;
  top: 15px;
  right: 5px;
  background-color: red;
  color: white;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  font-size: 12px;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* cart page footer style */
.cart_footer{
  display: flex;
  margin-bottom: 40%;
}
.cart_footer_spacer{
  width:70%;
}

.product_btn {
  width: 250px;
  border-radius: 4px;
  background-color: white;
  border: 2px solid black;
  overflow-x: auto;
  margin-left: 20px;
  
}

.checkout_btn {
  width: 250px;
  border-radius: 4px;
  background-color: #70cc84;
  border: none;
  margin-right: 20px;
}

/* footer style */
.footer {
  background-color: #132322;
  padding: 20px;
  color: white;
  align-items: flex-start;
}

.social {
  padding: 0;
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
  width: 20px;
  height: 20px;
  overflow: hidden;
  margin-top: 9px;
}

@media screen and (max-width: 1060px) {
  .footer {
    text-align: center;
  }

  .cart_footer{
    display: block;
  }

.product_btn {
  width: calc(100% - 100px);
  margin-left:40px;
}

.checkout_btn {
  width: calc(100% - 100px);
  margin-left:40px;
}


 
}
</style>
