<template>
  <section class="list-container">
    <h2>Pizza Menu Summary</h2>
    <ul>
      <li>Name : {{ data.name }}</li>
      <li>Phone : {{ data.phone }}</li>
      <li>Address : {{ data.address }}</li>
    </ul>

    <ul>
      <li>
        {{ data && data.pizzaSize && data.pizzaSize.name }} :
        {{ data && data.pizzaSize && data.pizzaSize.price }}
      </li>
      <li v-for="(item, index) in data.topping" :key="index">
        {{ item.name }} : {{ item.price }}
      </li>
      <li>TAX : {{ totalTax }}</li>
    </ul>

    <h1>Total : {{ total() }}</h1>
  </section>
</template>

<script>
export default {
  props: ["data"],
  data() {
    return {
      totalTax: null,
    };
  },
  methods: {
    total() {
      var total = 0;
      this.data.topping.forEach((element) => {
        if (!element.offerActive) {
          total += parseInt(element.price);
        }
      });
      total += parseInt(this.data.pizzaSize.price);
      var tax = (15 / 100) * total;
      total += tax;
      var totalRounded = Math.round(total);
      this.totalTax = Math.round(tax);
      return totalRounded;
    },
  },
};
</script>

<style>
.list-container {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  width: 50%;
  margin: 0 auto;
  box-shadow: 0 1.7em 5.5em -0.94em rgb(0 0 0 / 30%),
    0 2em 3em 0.5em rgb(0 0 0 / 10%), 0 1.8em 2em -1.5em rgb(0 0 0 / 20%);
  background-image: linear-gradient(
    -120deg,
    #26e289 0%,
    #1a5fa2 100%,
    #ff0000 200%
  );
  padding: 30px 50px;
}
.list-container ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.list-container h2 {
  font-size: 32px;
  padding-bottom: 20px;
  text-align: center;
  margin: 0;
}
@media screen and (max-width: 800px) {
    .list-container{
        width: 100%;
    }
}
</style>