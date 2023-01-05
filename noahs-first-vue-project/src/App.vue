<script setup>
import { ref } from "vue";

const userInput = ref("0");

const billCurrency = ref(Number(userInput.value).toFixed(2));

const numberOfGuests = ref(1);

const amountToPay = ref(0);

const serviceCharge = ref(0);

const evenSplit = ref(true);

const remainder = ref(0);

const oddOneOut = ref("");

function handleChange() {
  let slider = document.getElementsByClassName("slider")[0];
  numberOfGuests.value = Number(slider.value);
  calculateAmountToPay();
}

function changeServiceCharge(button) {
  let buttonClass = "--" + button;
  let clickedButton = document.getElementsByClassName(buttonClass)[0];
  let activeButton = document.getElementsByClassName("active")[0];
  activeButton.classList.remove("active");
  clickedButton.classList.add("active");
  serviceCharge.value = Number(button);
  calculateBill();
}

function handleClick(button) {
  if (button === "C") {
    userInput.value = "0";
  } else if (button === ".") {
    if (userInput.value.includes(".")) {
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
    return null;
  } else {
    userInput.value = userInput.value + button;
  }
  calculateBill();
  calculateAmountToPay();
}

function calculateBill() {
  let billWithoutTip = Number(userInput.value).toFixed(2);
  let tip = 0;
  if (serviceCharge.value) {
    tip = Number((billWithoutTip / 100) * serviceCharge.value).toFixed(2);
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
  amountToPay.value = Number(billCurrency.value / numberOfGuests.value).toFixed(
    2
  );
  if ( Number(amountToPay.value) * Number(numberOfGuests.value) !== Number(billCurrency.value)) {
    evenSplit.value = false;
    remainder.value = (Number(billCurrency.value)-(Number(amountToPay.value) * Number(numberOfGuests.value)));
    oddOneOut.value = Number(Number(amountToPay.value) + Number(remainder.value)).toFixed(2);
    if ( oddOneOut.value == amountToPay.value){
      evenSplit.value = true;
    }
  } else {
    evenSplit.value = true;
  }
}
</script>

<template className="app">
  <div className="app-container">
    <section className="heading-container">
      <h1 className="heading">SPLIT THE BILL</h1>
    </section>
    <section className="value-container">
      <section className="total-container">
        <h3 className="sub-heading">Total : £{{ billCurrency }}</h3>
      </section>
      <section className="sub-total-container">
        <!-- <h3 className="sub-heading">Sub-total : £{{ userInput }}</h3> -->
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
          <div />
          <button @click="handleClick('0')" className="grid-item">0</button>
          <div />
          <button @click="handleClick('C')" className="grid-item">C</button>
          <button @click="handleClick('.')" className="grid-item">.</button>
          <button @click="handleClick('del')" className="grid-item">del</button>
        </div>
      </section>
      <section className="service-container">
        <h3 className="service-sub-heading">Add a tip?</h3>
        <section className="service-charge-button-container">
          <button
            className="service-charge-button --0 active"
            @click="changeServiceCharge(0)"
          >
            0%
          </button>
          <button
            className="service-charge-button --5"
            @click="changeServiceCharge(5)"
          >
            5%
          </button>
          <button
            className="service-charge-button --10"
            @click="changeServiceCharge(10)"
          >
            10%
          </button>
          <button
            className="service-charge-button --12"
            @click="changeServiceCharge(12)"
          >
            12%
          </button>
          <button
            className="service-charge-button --15"
            @click="changeServiceCharge(15)"
          >
            15%
          </button>
          <button
            className="service-charge-button --20"
            @click="changeServiceCharge(20)"
          >
            20%
          </button>
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
      <section v-if="evenSplit" className="payment-container">
        <h3 className="sub-heading">Each Pays : £{{ amountToPay }}</h3>
      </section>
      <section v-else className="payment-container">
        <h3 className="sub-heading">One Pays : £{{ oddOneOut }}</h3>
        <h3 className="sub-heading">Rest Pay : £{{ amountToPay }}</h3>
      </section>
    </section>
  </div>
</template>

<style scoped>
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
  touch-action: manipulation;
}
.value-container {
  width: 100%;
  height: 95%;
  display: flex;
  flex-direction: column;
  background-color: var(--soft-color);
  align-items: center;
  justify-content: space-between;
}
.heading-container {
  background-color: var(--dark-color);
  width: 100%;
  height: 5%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bolder;
}
.heading {
  color: var(--highlight-color);
}
.sub-heading {
  font-size: 5.1vw;
}
.sub-total-container {
  background-color: var(--soft-color);
  color: var(--dark-color);
  width: 100%;
  height: 60%;
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
  grid-template-rows: repeat(5, 1fr);
}
.grid-item {
  border: none;
  background-color: var(--dark-color);
  clip-path: circle();
  border-radius: 50%;
  margin: 3%;
  color: var(--soft-color);
  font-size: 6vw;
  font-family: var(--app-font);
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
  justify-content: center;
  align-items: center;
}
.service-sub-heading {
  color: var(--highlight-color);
}
.total-container {
  width: 100%;
  display: flex;
  justify-content: space-evenly;
  color: var(--highlight-color);
  background-color: var(--dark-color);
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
  font-family: var(--app-font);
  font-size: 3.5vw;
  font-weight: bolder;
  transition: 0.3s;
}
.active {
  background-color: var(--highlight-color);
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
  width: 30px;
  height: 30px;
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
  height: 15%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
/* tablet and desktop screens */
@media only screen and (min-width: 650px) {
  .app-container {
    width: 35vw;
    height: 99vh;
    margin: 0;
  }
  .heading {
    font-size: 3vw;
  }
  .sub-heading {
    font-size: 2vw;
  }
  .grid-container {
    height: 30vw;
    width: 22.5vw;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(5, 1fr);
  }
  .grid-item {
    font-size: 2.1vw;
    cursor: pointer;
  }
  .grid-item:hover {
    color: var(--highlight-color);
    transition: color border 0.5s;
  }
  .service-charge-button {
    height: 70%;
    width: 16%;
    font-size: 1vw;
    cursor: pointer;
  }
}
</style>
