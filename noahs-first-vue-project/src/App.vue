<script setup>
import { ref } from "vue";

const userInput = ref("0");

const unsplitBill = ref(Number(userInput.value).toFixed(2));

const numberOfGuests = ref(1);

const amountToPay = ref(0);

const serviceCharge = ref(0);

const evenSplit = ref(true);

const remainder = ref(0);

const oddOneOut = ref("");

const buttonArray = ["1", "2", "3", "4", "5", "6", "7", "8", "9"];

//Called when the slider is moved.
function changeNumberOfGuests() {
  let slider = document.getElementsByClassName("slider")[0];
  numberOfGuests.value = Number(slider.value);
  calculateAmountToPay();
}

//Called within changeServiceCharge.
//This removes the class 'active' from the currently active button and adds it to the button that
//called the function, changing the background and text color so the user knows which service charge
//is being applied to the bill.
function setActiveButton(button) {
  let buttonClass = "--" + button;
  let clickedButton = document.getElementsByClassName(buttonClass)[0];
  let activeButton = document.getElementsByClassName("active")[0];
  activeButton.classList.remove("active");
  clickedButton.classList.add("active");
}

//Called when a service charge button is clicked. calls setActiveButton and
//updates the service charge state.
function changeServiceCharge(button) {
  setActiveButton(button);
  serviceCharge.value = Number(button);
  calculateBill();
}

function handleClick(button) {
  //ensures userInput always has a default value of "0", to avoid elements displaying as undefined or null;
  if (!userInput.value) {
    userInput.value = "0";
    return null;
  } else {
    //switch statement handles which button is being clicked;
    switch (button) {
      //if C button is clicked, reset userInput and serviceCharge to 0;
      case "C":
        userInput.value = "0";
        changeServiceCharge("0");
        break;
      //if "." button is clicked and userInput does not already contain ".", add ".";
      case ".":
        if (!userInput.value.includes(".")) {
          userInput.value += button;
        } else {
          return null;
        }
        break;
      // if del button is clicked, and userInput is not 0,
      //removes last character of string from userInput.
      case "del":
        if (userInput.value === "0") {
          userInput.value = "0";
        } else {
          userInput.value = userInput.value.slice(0, -1);
        }
        break;
      //default case handles buttons 0-9 being clicked
      default:
        //if userInput is currently 0 when button is clicked,
        //current button value replaces userInput.
        if (userInput.value === "0") {
          userInput.value = button;
          //if button is clicked and userInput is not already to 2 decimal places
          //or 7 characters in length, button value is added to end of string, otherwise
          //button click has no effect;
        } else if (
          !/^[0-9]+\.[0-9]{2,}/g.test(userInput.value) &&
          userInput.value.length <= 6
        ) {
          userInput.value += button;
        } else {
          return null;
        }
    }
  }
  //always calls calculateBill to update unsplitBill and amountToPay states;
  calculateBill();
}

function calculateBill() {
  let billWithoutTip = Number(userInput.value);
  //calculating tip as a percentage of the userInput
  let tip = Number((billWithoutTip / 100) * serviceCharge.value).toFixed(2);
  unsplitBill.value = Number(billWithoutTip + Number(tip)).toFixed(2);

  calculateAmountToPay();
}

function calculateUnevenSplit() {
  //state change triggers a conditional render of an additional element to highlight that
  //the bill is not split evenly.
  evenSplit.value = false;
  //calculates the amount left over from splitting the bill that is then added to the oddOneOut value.
  remainder.value =
    Number(unsplitBill.value) -
    Number(amountToPay.value) * Number(numberOfGuests.value);
  oddOneOut.value = Number(
    Number(amountToPay.value) + Number(remainder.value)
  ).toFixed(2);
  // fixes a bug where with some calculations, the bill was not being split evenly despite amount to pay and
  //odd one out having the same value.
  if (oddOneOut.value == amountToPay.value) {
    evenSplit.value = true;
  }
}

function calculateAmountToPay() {
  amountToPay.value = Number(unsplitBill.value / numberOfGuests.value).toFixed(
    2
  );
  //checks if bill can be split evenly between numberOfGuests. If not,
  //calls CalculateUnevenSplit.
  if (
    Number(amountToPay.value) * Number(numberOfGuests.value) !==
    Number(unsplitBill.value)
  ) {
    calculateUnevenSplit();
  } else {
    //evenSplit set back to true to trigger conditional render.
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
        <h3 className="sub-heading">Total : £{{ unsplitBill }}</h3>
      </section>
      <section className="sub-total-container">
        <div className="grid-container">
          <button
            v-for="value in buttonArray"
            :key="value"
            @click="handleClick(value)"
            className="grid-item"
          >
            {{ Number(value) }}
          </button>
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
            @input="changeNumberOfGuests"
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
