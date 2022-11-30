<script setup>
import { ref } from "vue";

const billTotal = ref("0");

const billCurrency = ref(Number(billTotal.value).toFixed(2));

const numberOfGuests = ref(1);

const amountToPay = ref(0);

function handleClick(button) {
  if (button === ".") {
    if (billTotal.value.includes(".")) {
      console.log(`${billTotal.value} includes .`);
      return null;
    } else {
      billTotal.value = billTotal.value + button;
    }
  } else if (!billTotal.value) {
    billTotal.value = "0";
  } else if (button == "del") {
    if (billTotal.value === "0") {
      billTotal.value = "0";
    } else {
      billTotal.value = billTotal.value.slice(0, -1);
      if (!billTotal.value) {
        billTotal.value = "0";
      }
    }
  } else if (billTotal.value === "0") {
    billTotal.value = button;
  } else if (/^[0-9]+\.[0-9]{2,}/g.test(billTotal.value)) {
    console.log("its a match");
    return null;
  } else {
    billTotal.value = billTotal.value + button;
  }
  calculateAmountToPay();
  convertInputToCurrency();
  console.log(`${billTotal.value}`);
}

function convertInputToCurrency() {
  billCurrency.value = Number(billTotal.value).toFixed(2);
}

function calculateAmountToPay() {
  amountToPay.value = Number(billTotal.value / numberOfGuests.value).toFixed(2);
}

function divideBy(number){
  numberOfGuests.value = number;
  calculateAmountToPay();
}
</script>

<template>
  <body className="body">
    <section className="heading-container">
      <h1 className="heading">Split the Bill!</h1>
    </section>
    <section className="value-container">
      <h3>Bill : £{{ billTotal }}</h3>
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
    <section className="division-container">
      <button @click="divideBy(1)" className="division-button">1</button>
      <button @click="divideBy(2)" className="division-button">2</button>
      <button @click="divideBy(3)" className="division-button">3</button>
      <button @click="divideBy(4)" className="division-button">4</button>
      <button @click="divideBy(5)" className="division-button">5</button>
    </section>
  </body>
</template>

<style scoped>
.count-button {
  color: blue;
}
.body {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
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
</style>
