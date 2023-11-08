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
        />
      </section>
    </section>
    <section id="CheckoutForm">
      <h2>Costumer Information</h2>
      <p>Fill out your billing and delivery information</p>
      <h3>Delivery Information</h3>
      <form>
        <p>
          <label for="firstname">Full Name</label><br />
          <input
            type="text"
            id="fullname"
            name="fn"
            required="required"
            placeholder="First- and Last name"
          />
        </p>
        <p>
          <label for="email">E-mail</label><br />
          <input
            type="text"
            id="email"
            name="em"
            required="required"
            placeholder="E-mail address"
          />
        </p>
        <p>
          <label for="street">Street</label><br />
          <input
            type="text"
            id="street"
            name="s"
            required="required"
            placeholder="Street name"
          />
        </p>
        <p>
          <label for="house">House</label><br />
          <input
            type="number"
            id="house"
            name="h"
            required="required"
            placeholder="House number"
          />
        </p>
        <p>
          <label for="payment">Choose Payment Option</label>
          <br />
          <select id="payment" name="pmt">
            <option selected="selected">Credit Card</option>
            <option>Swish</option>
            <option>Klarna</option>
            <option>Pay with app credits</option>
          </select>
        </p>
        <p>
          <label for="gender">Select Gender</label>
          <br />
          <label for="male">Male</label>
          <input
            type="radio"
            id="male"
            name="gender"
            checked="checked"
            value="Male"
          />
          <br />
          <label for="female">Female</label>
          <input type="radio" name="gender" id="female" value="Female" />
          <br />
          <label for="do not wish to provide">Do not wish to provide</label>
          <input
            type="radio"
            name="gender"
            id="not provide"
            value="Do not wish to provide"
          />
        </p>
      </form>
    </section>
    <button type="submit">
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
import menu from '../assets/menu.json';

const socket = io();

function MenuItem(cal, nm, url, gluten, lactose) {
  this.name = nm;
  this.kCal = cal;
  this.imageURL = url;
  this.containsGluten = gluten;
  this.containsLactose = lactose;
}

let hellBurger = new MenuItem(
  1000,
  "HellBurger",
  "https://www.ocregister.com/wp-content/uploads/2022/03/OCR-L-FirstStBurger-WBOX-0310-1-1.jpg?w=620",
  true,
  true
);

let heavenBurger = new MenuItem(
  10,
  "HeavenBurger",
  "https://2.bp.blogspot.com/-iuA4duilEos/TvSUrELIbVI/AAAAAAAAAVI/Q42RuNGuFPM/s1600/Screen+shot+2011-12-23+at+9.43.45+AM.png",
  false,
  false
);

let leprechaunBurger = new MenuItem(
  500,
  "LeprechaunBurger",
  "https://www.burger.vn/wp-content/uploads/2023/05/NAshville-burger-BCB-burgers-Saigon-VIetnam-1-1-500x400.png",
  true,
  false
);

const burgerArray = [hellBurger ,  heavenBurger ,  leprechaunBurger ];

export default {
  name: "HomeView",
  components: {
    Burger,
  },
  data: function () {
    return {
      burgers: menu
    };
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: event.clientX - 10 - offset.x,
          y: event.clientY - 10 - offset.y,
        },
        orderItems: ["Beans", "Curry"],
      });
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
  text-align:center;
  margin-left:-300px;
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

button {
  margin: 20px 0px 10px 40px;
}

section {
  margin-left: 15px;
  margin-right: 15px;
  padding-left: 20px;
}
</style>
