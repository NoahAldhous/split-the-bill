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
    billCurrency.value = Number(billCurrency.value).toFixed(2);
  } else {
    billCurrency.value = Number(billWithoutTip);
    billCurrency.value = Number(billCurrency.value).toFixed(2);
  }
  calculateAmountToPay();
}

function calculateAmountToPay() {
  amountToPay.value = Number(billCurrency.value / numberOfGuests.value).toFixed(2);
}
</script>

<template className="app">
  <div className="app-container">
    <section className="heading-container">
      <h1 className="heading">SPLIT THE BILL</h1>
    </section>
    <section className="value-container">
      <section className="sub-total-container">
        <h3 className="sub-heading">Sub-total : £{{ userInput }}</h3>
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
      <section className="service-container">
        <section className="service-charge-button-container">
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
        <section className="service-text-container">
          <h3 className="sub-heading">Service : {{ serviceCharge }}%</h3>
          <h3 className="sub-heading">Total : £{{ billCurrency }}</h3>
        </section>
      </section>
      <section className="split-container">
        <h3 className="sub-heading">Split Between : {{ numberOfGuests }}</h3>
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
      </section>
      <section className="payment-container">
        <h3 className="sub-heading">Each Pays : £{{ amountToPay }}</h3>
      </section>
    </section>
  </div>
</template>

<style scoped>
.app {
  padding-top: 20%;
}
.app-container {
  width: 95vw;
  height: 85vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: var(--background-color);
  border: 2px solid var(--dark-color);
  margin-top: 10%;
  margin-bottom: 20%;
}
.value-container {
  width: 100%;
  height: 93%;
  display: flex;
  flex-direction: column;
  background-color: var(--soft-color);
  align-items: center;
  justify-content: space-between;
}
.heading-container {
  background-color: var(--dark-color);
  width: 100%;
  height: 7%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bolder;
}
.heading {
  color: var(--highlight-color);
}
.sub-heading {
  font-size: 6vw;
}
.sub-total-container {
  background-color: var(--soft-color);
  color: var(--dark-color);
  width: 100%;
  height: 65%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.grid-container {
  height: 80vw;
  width: 60vw;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(4, 1fr);
}
.grid-item {
  border: none;
  background-color: var(--dark-color);
  clip-path: circle();
  border-radius: 50%;
  margin: 5%;
  color: var(--soft-color);
  font-size: 6vw;
  display: flex;
  align-items: center;
  justify-content: center;
}
.service-container {
  background-color: var(--dark-color);
  color: var(--soft-color);
  width: 100%;
  height: 15%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.service-text-container {
  width: 100%;
  display: flex;
  justify-content: space-evenly;
}
.service-charge-button-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 95%;
  height: 60%;
}
.service-charge-button {
  border: none;
  padding: 0;
  background-color: var(--soft-color);
  color: var(--dark-color);
  width: 15%;
  height: 50%;
  box-shadow: 1px 1px 1px 1px black;
  border-radius: 10px;
  font-size: 4vw;
  font-weight: bolder;
}
.split-container {
  background-color: var(--soft-color);
  color: var(--dark-color);
  width: 85%;
  height: 10%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.slider-container {
  width: 100%;
}

.slider {
  -webkit-appearance: none; /* Override default CSS styles */
  appearance: none;
  width: 100%; /* Full-width */
  height: 15px;
  border-radius: 5px; 
  background: var(--dark-color); /* Grey background */
  outline: none; /* Remove outline */
  -webkit-transition: 0.2s; /* 0.2 seconds transition on hover */
  transition: opacity 0.2s;
}
.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  background: var(--highlight-color);
  cursor: pointer;
}
.slider::-moz-range-thumb {
  width: 25px;
  height: 25px;
  border-radius: 50%;
  background: var(--hightlight-color);
  cursor: pointer;
}
.payment-container {
  background-color: var(--dark-color);
  color: var(--highlight-color);
  width: 100%;
  height: 5%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
