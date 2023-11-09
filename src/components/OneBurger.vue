<template>
  <div class="{{ burger.name }}" style="display: inline-block">
    <h3 class="BurgerName">{{ burger.name }}</h3>
    <img v-bind:src="burger.imageURL" />
    <section>
      <p class="burgerDescription">In Cart: {{ amountOrdered }}</p>
      <button v-on:click="addOneOrder()">+</button>
      <button v-on:click="removeOneOrder()">-</button>
    </section>
    <section class="burgerDescription">
      <ul>
        <li>kCalories: {{ burger.kCal }}</li>
        <li v-if="burger.containsGluten">Contains Gluten</li>
        <li v-if="burger.containsLactose">Contains Lactose</li>
      </ul>
    </section>
  </div>
</template>

<script>
export default {
  name: "OneBurger",
  props: {
    burger: Object,
  },
  data: function () {
    return {
      amountOrdered: 0,
    };
  },
  methods: {
    addOneOrder: function () {
      this.amountOrdered++;
      this.$emit("orderedBurger", {
        name: this.burger.name,
        amount: this.amountOrdered,
      });
    },
    removeOneOrder: function () {
      if (this.amountOrdered != 0) {
        this.amountOrdered--;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
