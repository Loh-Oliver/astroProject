<template>
  <div>
    <p>Item ID: {{ itemId }}</p>
    <div></div>
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
  mounted() {
    this.fetchProducts(2);
  },
  methods: {},
};
</script>

<style scoped>
/* Add your scoped styles here */
</style>
