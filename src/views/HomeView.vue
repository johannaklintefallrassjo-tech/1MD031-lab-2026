<template>

        <header>
        <h1><strong>Välkommen till Jojs Burgir</strong></h1>
    
        </header>
        <main>
        <section>
        <h2>Välj Burgir</h2>
                <p>Välj din burgir nedan</p>
        </section>

        <section id="burgers">

        <div class="wrapper">

               <Burger v-for="burger in burgers"
            v-bind:burger="burger"
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"
            />

        </div>

        </section>

        <section id="order">
        <h2>Customer Information</h2>
                <p>Provide necassary information</p>
        </section>
 <form>
                <section id="contact">
                                <h3>Delievery Address</h3>

       
        <p>
        <label for="full name">Full name</label><br>
        <input type="text" id="fullname" v-model="fullname" required="required" placeholder="full name">
        </p>
        <p>
        <label for="e-mailadress">e-mailadress</label><br>
        <input type="text" id="email" v-model="email" placeholder="e-mailaddress">
        </p>
     

        </section>

    <section>
        <p>
   <label for="recipient">Recipient</label>br>
   <select id="recipient" v-model="recipient">
       <option>Apple Pay</option>
       <option>Kort</option>
       <option>Klarna</option>
       <option>Swish</option>
   </select>
</p>
</section>

<section>

       <p>Gender</p> 
       <input type="radio" id="female" v-model="gender" value="female">
<label for="female">Female</label><br>
<input type="radio" id="male" v-model="gender" value="male">
<label for="male">Male</label><br>
<input type="radio" id="noanswer" v-model="gender" value="noanswer">
<label for="noanswer">Chose not to answer</label>
</section>

<section>
<button type="button" v-on:click="placeOrder">
  <img src="/img/place_order.jpg" style="width:30px;height:30px" >
  Place order
</button>

</section>

        </form>
    

        </main>


        <footer>
                <h3>
                Contact
                </h3>
                        <div class="footer">

<button type="submit">
                info@jojsburger.com
</button>
        <button type="submit">
                Jojs Gata 67 
</button>
<button type="submit">
                0701234567
</button>

                        </div>
        </footer>
    

<div>
    
   <div id="map-container">
    <div id="map" v-on:click="setLocation">
<div id="target-dot" v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }">
      T
    </div>
    </div>
</div>
</div>
</template>

<script>

import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'


const socket = io("localhost:3000");

function MenuItem(name, url, kCal, gluten, lactose) {
    this.name = name;
    this.url = url;
    this.kCal = kCal;
    this.gluten = gluten;
    this.lactose = lactose;
}

const burgers = [
    new MenuItem("Sojs de bearnojs burgir", "/img/sojsdebearnojse.jpg", 650, true, true),
    new MenuItem("Chicken Parmesan Burgir", "/img/chickenparm.jpg", 550, true, true),
    new MenuItem("Flygande Burgare", "/img/flygandeburgare.jpg", 600, true, false)
];

console.log(burgers);

export default {
  name: 'HomeView',
  components: {
    Burger
  },
 data: function () {
    return {
      burgers: menu,
      fullname: "",
    email: "",
    recipient: "Apple Pay",
    gender: "",
    orderedBurgers: {},
    location: { x: 0,
            y: 0
          }
    }
  },
  methods: {
    placeOrder: function () {
      console.log(
        this.fullname,
        this.email,
        this.recipient,
        this.gender,
        this.orderedBurgers
      );
      socket.emit("addOrder", {
    orderId: this.getOrderNumber(),
    details: {
      x: this.location.x,
      y: this.location.y
    },
    orderItems: this.orderedBurgers,
    customer: {
          name: this.fullname,
          email: this.email,
          payment: this.recipient,
          gender: this.gender
    }
  });
},
  

    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addToOrder: function (event) {
  this.orderedBurgers[event.name] = event.amount;
  },
  setLocation: function (event) {
  var offset = {
    x: event.currentTarget.getBoundingClientRect().left,
    y: event.currentTarget.getBoundingClientRect().top
  };
  this.location.x = event.clientX - 10 - offset.x;
  this.location.y = event.clientY - 10 - offset.y;
},
   
  }

}


</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
body {
    font-size: 10pt;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
            font-weight: 100;
                    text-transform: uppercase;


    color: blue;
            text-shadow: 0 0 2px  black;

}

p {
    color: red;
}

h1 {
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    font-size: 36pt;
        color: red;
        text-shadow: 0 0 5px  black;
        text-transform: uppercase;
        font-weight: 900;
        alignment-baseline: center;


}
main, header, footer, nav ul {
    max-width: 80rem;
    margin: 0 auto 0 auto;
}
main {
    background-color: rgb(255, 246, 233);
}

/* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} */

header {
    background-image: url("../img/header_burger.jpg");
    background-size: cover;
    overflow: hidden;
    width: 100%;
    height: 200px;
    background-image: linear-gradient(rgba(255, 255, 255, 0.5), rgba(255, 255, 255, 0.5)), url("../img/header_burger.jpg");
    display: flex;
    justify-content: center;
    align-items: center;

}

header h1 {
    width:40rem;
    margin: 0 auto;
    text-align: center;

}

nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}

nav li {
    display: block;
    background-color: grey;
    padding: 1em;
}

.Very-good {
    color: green;
}

.Master {
    color: green;
    font-weight: bold;
}

.allergen {
    font-weight: bold;
}


#burgers div{
    background-color: red;
    color: white;
    padding: 5px;
    margin: 15px;
    padding: 15px;
    border: 7px dotted;
    border-color: beige;
}

#order {
    background-color: red;
    color: white;
}

button:hover {
   background-color: rgb(255, 253, 253);
   cursor: pointer;
}

/* 1. Luft runt dina sektioner så texten inte ligger kloss mot kanten */
section {
    margin: 20px;
}

/* 2. Luft ovanför/runt knappen så den inte sitter fast i sektionen ovanför */
button {
    margin: 20px 20px 20px 20px
}

.wrapper {
     display: grid;
     grid-gap: 10px;
     grid-template-columns: 33% 33% 33%;

 }

 .footer {
     display: grid;
     grid-gap: 10px;
     grid-template-columns: 33% 33% 33%;

 }
#map-container {
  width: 100%;
  height: 500px;
  overflow: scroll;
}

#map {
  width: 1920px;
  height: 1078px;
  background: url("/img/polacks.jpg");
  position: relative;
}

#target-dot {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}


</style>