<template>
  <div class="container">
    <div class="inner-container">
      <Model v-if="model" :data="dataObj" />
      <div class="main-heading">
        <h2>Pizza Menu</h2>
      </div>
      <!-- step one -->
      <section v-if="currentStep == 1">
        <h1>Choose Pizza Size</h1>
        <div v-for="(item, index) in sizesList" :key="index">
          <label
            ><input type="radio" :value="item" v-model="pizzaSize" />
           <div class="price">
             <span> {{ item.name }}</span> <span>${{ item.price }}</span>
           </div>
          </label>
        </div>
      </section>

      <!-- step two -->
      <section v-if="currentStep == 2">
        <h1>Choose Your Topping</h1>
        <div v-for="(item, index) in toppingList" :key="index">
          <label
            ><input
              type="checkbox"
              :value="item"
              v-model="topping"
              :disabled="item.name === oneIngredient && isActive"
              @click="itemSelected(item)"
            />
           <div class="price">
             <span> {{ item.name }}</span> <span>${{ !item.offerActive ? item.price : "Free" }}</span>
           </div>
          </label>
        </div>
      </section>

      <!-- step three -->
      <section v-if="currentStep == 3">
        <h1>Customer Details</h1>
        <div class="input-style">
          <input type="text" v-model="name" placeholder="Name" />
          <input type="number" v-model="phone" placeholder="Phone" />
          <input type="text" v-model="address" placeholder="Address" />
        </div>
      </section>

      <div class="btn-container">
        <button class="btn" @click="prevSteps" :disabled="currentStep == 1">
          Prev
        </button>
        <button
          class="btn"
          v-if="currentStep != 3"
          @click="checkSteps"
          :disabled="
            currentStep == 1 && !pizzaSize
              ? true
              : false || (currentStep == 2 && topping.length < 6)
              ? true
              : false
          "
        >
          Next
        </button>
        <button
          class="btn"
          v-else
          :disabled="!name || !phone || !address"
          @click="placeOrder"
        >
          Place Order
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Model from "./model/model.vue";
export default {
  name: "Pizza",
  components: {
    Model,
  },
  data() {
    return {
      arr: [],
      model: false,
      pizzaSize: null,
      topping: [],
      name: null,
      phone: null,
      address: null,
      dataObj: {},
      //   user for data
      currentStep: 1,
      oneIngredient: null,
      isActive: false,
      buttonDisable: true,
      toppingList: [
        { name: "Mashromes", price: "1", offerActive: false },
        { name: "Olives", price: "2", offerActive: false },
        { name: "Tomato", price: "3", offerActive: true },
        { name: "Tona", price: "5", offerActive: false },
        { name: "Pineapple", price: "6", offerActive: false },
        { name: "Seafood", price: "7", offerActive: false },
        { name: "Pepperoni", price: "8", offerActive: false },
        { name: "Bacon", price: "9", offerActive: false },
        { name: "Onion", price: "10", offerActive: false },
        { name: "Mozzarella", price: "11", offerActive: true },
      ],
      sizesList: [
        { name: "Small", price: "5" },
        { name: "Medium", price: "10" },
        { name: "Large", price: "15" },
        { name: "Extra Large", price: "20" },
      ],
      select: "",
      offerRemove: [],
    };
  },
  methods: {
    checkSteps() {
      if (this.currentStep == 1) {
        this.currentStep = 2;
      } else if (this.currentStep == 2) {
        this.currentStep = 3;
      }
      if (this.pizzaSize) {
        this.buttonDisable = false;
      }
    },
    prevSteps() {
      if (this.currentStep == 2) {
        this.currentStep = 1;
      } else if (this.currentStep == 3) {
        this.currentStep = 2;
      }
    },
    placeOrder() {
      this.dataObj = {
        pizzaSize: this.pizzaSize,
        topping: this.topping,
        name: this.name,
        phone: this.phone,
        address: this.address,
      };
      this.model = true;
    },
    itemSelected(e) {
      this.toggle(e);
      if (e.name == "Mozzarella" || e.name == "Bacon") {
        this.isActive = !this.isActive;
        if (e.name == "Mozzarella") {
          this.oneIngredient = "Bacon";
        } else if (e.name == "Bacon") {
          this.oneIngredient = "Mozzarella";
        }
      }
    },
    toggle(e) {
      if (this.arr.some((vExist) => vExist.name === e.name)) {
        var x = this.arr.filter((v) => v.name != e.name);
        this.arr = x;
        var filterSameValues = this.arr.filter((v) => v.offerActive != true);
        if (filterSameValues) {
          if (filterSameValues.length == 1 || filterSameValues.length == 3) {
            var max = Math.max(...this.offerRemove.map((item) => item.price));
            if (max) {
              var indexMax = this.toppingList.findIndex(
                (index) => index.price == max
              );
              this.toppingList[indexMax].offerActive = false;
              var re = this.offerRemove.filter((v) => v.price != max);
              this.offerRemove = re;
            }
          }
        }
      } else {
        this.arr.push(e);
        var uniqueArray = this.toppingList.filter(
          (val) => !this.arr.includes(val)
        );
        var a = this.arr.filter((v) => v.offerActive != true);
        var u = uniqueArray.filter((v) => v.offerActive != true);
        if (e.offerActive == false) {
          if (a.length == 2 || a.length == 4) {
            var min = Math.min(...u.map((item) => item.price));
            if (min) {
              var indexOfmin = this.toppingList.findIndex(
                (index) => index.price == min
              );
              this.toppingList[indexOfmin].offerActive = true;
              this.offerRemove.push(this.toppingList[indexOfmin]);
            }
          }
        }
      }
    },
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
.container .inner-container {
  width: 30%;
  /* background-color: #36454f; */
  box-shadow: 0 1.7em 5.5em -0.94em rgb(0 0 0 / 30%),
    0 2em 3em 0.5em rgb(0 0 0 / 10%), 0 1.8em 2em -1.5em rgb(0 0 0 / 20%);
  background-image: linear-gradient(
    -120deg,
    #26e289 0%,
    #1a5fa2 100%,
    #ff0000 200%
  );
  padding: 30px 50px 50px 50px;
  height: 100%;
  margin-top: 30px;
}
.container .inner-container .main-heading h2 {
  font-size: 32px;
  padding-bottom: 20px;
  text-align: center;
  margin: 0;
}
.container .inner-container h1 {
  margin: 0;
  font-size: 20px;
  padding-bottom: 10px;
}
label{
  display: flex;
  cursor: pointer;
  align-items: baseline;
}
.container .inner-container .price{
 width: 90%;
 display: flex;
 justify-content: space-between;
}
.container .inner-container .price span:nth-child(2){
  width: 30px;
  text-align: left;
}
.container .inner-container .btn-container {
  width: 100%;
  display: flex;
  justify-content: space-between;
  margin-top: 15px;
}
.container .inner-container .btn-container .btn {
  font-size: 14px;
  padding: 10px 40px;
  color: #fff;
  background-color: #37af65;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}
.container .inner-container .input-style {
  display: flex;
  flex-direction: column;
}
.container .inner-container .input-style input {
  margin: 10px 0;
  font-size: 14px;
  height: 35px;
  padding: 5px 10px;
  color: #495057;
  background-color: #fff;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}
.container .inner-container .input-style input:focus {
  border-color: #80bdff;
  outline: 0;
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}
@media screen and (max-width: 800px) {
  .container{
    height: 100vh!important;
    margin: 15px;
  }
  .container .inner-container{
    width: 100%;
    height: fit-content;
  }
}
</style>