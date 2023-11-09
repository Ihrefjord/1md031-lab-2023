<template>
  <header>
    <div id="HeadlineAndImage">
      <img
        id="HeaderImage"
        src="https://images.alphacoders.com/131/1319547.png"
        alt="Fantasy Land"
        title="Picture of fantasy land"
      />
      <h1>Welcome to Fantasy Burger Online</h1>
    </div>
  </header>
  <main>
    <section id="BurgerSelection">
      <h2>Select your burger</h2>
      <p>Choose from the three magical options below</p>
      <section id="burgerDisplay">
        <Burger
          v-for="burger in burgers"
          v-bind:key="burger.name"
          v-bind:burger="burger"
          v-on:orderedBurger="addToOrder($event)"
        />
      </section>
    </section>
    <section id="CheckoutForm">
      <h2>Costumer Information</h2>
      <p>Fill out your billing and delivery information</p>
      <h3>Delivery Information</h3>
      <form>
        <p>Full Name</p>
        <input
          v-model="costumerInformation.fullname"
          placeholder="First- and Last name"
        />

        <p>E-mail</p>
        <input
          v-model="costumerInformation.email"
          placeholder="E-mail address"
        />

        <p>Choose Payment Option:</p>
        <select v-model="costumerInformation.payment">
          <option>Credit Card</option>
          <option>Swish</option>
          <option>Klarna</option>
          <option>Pay with app credits</option>
        </select>

        <p>Select Gender</p>
        <label for="male">Male</label>
        <input
          type="radio"
          id="male"
          v-model="costumerInformation.gender"
          value="Male"
        />
        <br />
        <label for="female">Female</label>
        <input
          type="radio"
          v-model="costumerInformation.gender"
          id="female"
          value="Female"
        />
        <br />
        <label for="do not wish to provide">Do not wish to provide</label>
        <input
          type="radio"
          v-model="costumerInformation.gender"
          id="not provide"
          value="Do not wish to provide"
        />
      </form>
    </section>

    <section id="mapBox">
      <div id="map" v-on:click="setLocation">
        <div
          v-bind:style="{
            left: this.location.x + 'px',
            top: this.location.y + 'px',
          }"
        >
          T
        </div>
      </div>
    </section>
    <button type="submit" v-on:click="submitOrder">
      <img
        src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/Eo_circle_green_checkmark.svg/1200px-Eo_circle_green_checkmark.svg.png"
        style="width: 20px"
      />
      Send Info
    </button>
  </main>
  <hr />
  <footer>&copy; 2023 Fantasy Burgers Inc All rights Reserved</footer>
</template>

<script>
import Burger from "../components/OneBurger.vue";
import io from "socket.io-client";
import menu from "../assets/menu.json";
import { ref } from "vue";

const socket = io();

const fullname = ref("Hej");

function MenuItem(cal, nm, url, gluten, lactose) {
  this.name = nm;
  this.kCal = cal;
  this.imageURL = url;
  this.containsGluten = gluten;
  this.containsLactose = lactose;
}

const hellBurger = new MenuItem(
  1000,
  "HellBurger",
  "https://www.ocregister.com/wp-content/uploads/2022/03/OCR-L-FirstStBurger-WBOX-0310-1-1.jpg?w=620",
  true,
  true
);

const heavenBurger = new MenuItem(
  10,
  "HeavenBurger",
  "https://2.bp.blogspot.com/-iuA4duilEos/TvSUrELIbVI/AAAAAAAAAVI/Q42RuNGuFPM/s1600/Screen+shot+2011-12-23+at+9.43.45+AM.png",
  false,
  false
);

const leprechaunBurger = new MenuItem(
  500,
  "LeprechaunBurger",
  "https://www.burger.vn/wp-content/uploads/2023/05/NAshville-burger-BCB-burgers-Saigon-VIetnam-1-1-500x400.png",
  true,
  false
);

export default {
  name: "HomeView",
  components: {
    Burger,
  },
  data: function () {
    return {
      burgers: menu,

      costumerInformation: {
        fullname: "",
        email: "",
        payment: "",
        gender: "",
      },

      orderedBurgers: {},

      location: {
        x: 0,
        y: 0,
      },
    };
  },
  methods: {
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    },
    submitOrder: function () {
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y,
          name: this.costumerInformation.fullname,
          email: this.costumerInformation.email,
          payment: this.costumerInformation.payment,
          gender: this.costumerInformation.gender,
        },
        orderItems: this.orderedBurgers,
      });
    },
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
  },
};
</script>

<style>
@import "https://fonts.googleapis.com/css?family=Pacifico|Dosis";

@media screen and (max-width: 800px) {
  h1 {
    font-size: 6vw;
  }
}

body {
  font-family: Georgia, "Times New Roman", Times, serif;
  font-size: 1.5em;
}

#HeadlineAndImage {
  margin-left: 20px;
  margin-right: 20px;

  height: 200px;
  overflow: hidden;
  margin-bottom: 10px;
}

#HeaderImage {
  opacity: 0.5;
  width: 100%;
  height: auto;
}

h1 {
  position: absolute;
  margin-top: -750px;
  margin-left: 370px;
}

#BurgerSelection {
  color: darkgreen;
  border: 2px dashed darkgreen;
}

#burgerDisplay {
  margin-left: -6px;
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 600px 600px 500px;
  background-color: darkgrey;
}

#burgerDisplay img {
  height: auto;
  width: 300px;
}

.HellBurger {
  grid-column: 1;
  align-self: center;
}

.Heavenburger {
  grid-column: 2;
  align-self: center;
}

.LeprechaunBurger {
  grid-column: 3;
  align-self: center;
}

.BurgerName {
  text-align: center;
  margin-left: -300px;
}

.burgerDescription {
  text-align: left;
  font-size: medium;
}

.burgerDisplay {
  margin-right: 100px;
}

#Allergies {
  font-weight: bold;
  color: black;
}

#allergyList {
  color: red;
}

#CheckoutForm {
  background-color: black;
  color: white;
  border: 2px dashed white;
}

button:hover,
input[type="radio"] {
  background-color: lightslategrey;
  cursor: pointer;
}

#map {
  cursor: crosshair;
  height: 1078px;
  width: 1920px;
  position: relative;
  background: url("../../public/img/polacks.jpg");
}

#map div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}

#mapBox {
  overflow: scroll;
  height: 400px;
  width: auto;
}

button {
  margin: 20px 0px 10px 40px;
}

section {
  margin-left: 15px;
  margin-right: 15px;
  padding-left: 20px;
}
</style>
