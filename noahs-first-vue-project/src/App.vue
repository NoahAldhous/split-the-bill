<script setup>
import { ref } from "vue";

const userInput = ref("0");

const billCurrency = ref(Number(userInput.value).toFixed(2));

const numberOfGuests = ref(1);

const amountToPay = ref(0);

const serviceCharge = ref(0);

function handleChange() {
  let slider = document.getElementsByClassName("slider")[0];
  console.log(slider);
  console.log(slider.value);
  numberOfGuests.value = Number(slider.value);
  calculateAmountToPay();
}

function changeServiceCharge(button){
  serviceCharge.value = Number(button);
  calculateBill();
}

function handleClick(button) {
  if (button === ".") {
    if (userInput.value.includes(".")) {
      console.log(`${userInput.value} includes .`);
      return null;
    } else {
      userInput.value = userInput.value + button;
    }
  } else if (!userInput.value) {
    userInput.value = "0";
  } else if (button == "del") {
    if (userInput.value === "0") {
      userInput.value = "0";
    } else {
      userInput.value = userInput.value.slice(0, -1);
      if (!userInput.value) {
        userInput.value = "0";
      }
    }
  } else if (userInput.value === "0") {
    userInput.value = button;
  } else if (/^[0-9]+\.[0-9]{2,}/g.test(userInput.value)) {
    console.log("its a match");
    return null;
  } else {
    userInput.value = userInput.value + button;
  }
  calculateAmountToPay();
  calculateBill();
}

function calculateBill() {
  let billWithoutTip = Number(userInput.value).toFixed(2);
  let tip = 0;
  if (serviceCharge.value) {
    tip = Number((billWithoutTip / 100) * serviceCharge.value).toFixed(2);
    console.log(tip);
    Number(tip);
    billCurrency.value = Number(billWithoutTip) + Number(tip);
    Number(billCurrency.value).toFixed(2);
  } else {
    billCurrency.value = Number(billWithoutTip);
    Number(billCurrency.value).toFixed(2);
  }
  calculateAmountToPay();
}

function calculateAmountToPay() {
  amountToPay.value = Number(billCurrency.value / numberOfGuests.value).toFixed(2);
}
</script>

<template>
  <div className="app-container">
    <section className="heading-container">
      <h1 className="heading">Split the Bill!</h1>
    </section>
    <section className="value-container">
      <h3>User Input : £{{ userInput }}</h3>
      <h3>Service : {{ serviceCharge }}%</h3>
      <section className="service-charge-container">
        <button
          className="service-charge-button"
          @click="changeServiceCharge(0)"
        >
          0%
        </button>
        <button
          className="service-charge-button"
          @click="changeServiceCharge(5)"
        >
          5%
        </button>
        <button
          className="service-charge-button"
          @click="changeServiceCharge(10)"
        >
          10%
        </button>
        <button
          className="service-charge-button"
          @click="changeServiceCharge(12)"
        >
          12%
        </button>
        <button
          className="service-charge-button"
          @click="changeServiceCharge(15)"
        >
          15%
        </button>
        <button
          className="service-charge-button"
          @click="changeServiceCharge(20)"
        >
          20%
        </button>
      </section>
      <h3>Bill : £{{ billCurrency }}</h3>
      <h3>Split Between : {{ numberOfGuests }}</h3>
      <h3>To Pay : £{{ amountToPay }}</h3>
    </section>
    <section className="numpad-container">
      <div className="grid-container">
        <button @click="handleClick('1')" className="grid-item">1</button>
        <button @click="handleClick('2')" className="grid-item">2</button>
        <button @click="handleClick('3')" className="grid-item">3</button>
        <button @click="handleClick('4')" className="grid-item">4</button>
        <button @click="handleClick('5')" className="grid-item">5</button>
        <button @click="handleClick('6')" className="grid-item">6</button>
        <button @click="handleClick('7')" className="grid-item">7</button>
        <button @click="handleClick('8')" className="grid-item">8</button>
        <button @click="handleClick('9')" className="grid-item">9</button>
        <button @click="handleClick('.')" className="grid-item">.</button>
        <button @click="handleClick('0')" className="grid-item">0</button>
        <button @click="handleClick('del')" className="grid-item">del</button>
      </div>
    </section>
    <section className="slider-container">
      <input
        type="range"
        min="1"
        max="10"
        value="1"
        className="slider"
        @input="handleChange"
      />
    </section>
  </div>
</template>

<style scoped>
.app-container {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: gray;
}
.numpad-container {
  height: 50%;
  width: 50%;
}
.grid-container {
  border: 1px solid red;
  height: 100%;
  width: 100%;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(4, 1fr);
}
.grid-item {
  border: 1px solid blue;
}

.slider-container {
  width: 50%;
}

.slider {
  -webkit-appearance: none; /* Override default CSS styles */
  appearance: none;
  width: 100%; /* Full-width */
  height: 25px; /* Specified height */
  background: #d3d3d3; /* Grey background */
  outline: none; /* Remove outline */
  opacity: 0.7; /* Set transparency (for mouse-over effects on hover) */
  -webkit-transition: 0.2s; /* 0.2 seconds transition on hover */
  transition: opacity 0.2s;
}
</style>
