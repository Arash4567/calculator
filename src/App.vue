<script setup>
import ButtonInput from "./components/ButtonInput.vue";
import CalculateDisplay from "./components/CalculateDisplay.vue";
import { ref } from "vue";
const histroryOperation = ref([]);
const currentCalculate = ref("0");
const lastOperation = ref("0");

const getPercent = () => {
  let percent = currentCalculate.value.replace(",", ".") / 100;
  currentCalculate.value = percent.toString().replace(".", ",");
};
const allClear = () => {
  currentCalculate.value = "0";
  lastOperation.value = "0";
};
const doDelete = () => {
  if (currentCalculate.value !== "0") {
    currentCalculate.value = currentCalculate.value.toString().slice(0, -1);
  }
  if (currentCalculate.value === "") {
    currentCalculate.value = "0";
  }
};
const inputValue = (val) => {
  if (val === "0") {
    if (currentCalculate.value === "0") {
      currentCalculate.value = "0";
    } else {
      currentCalculate.value += maskedIn(val);
    }
  } else if (val === "00") {
    if (currentCalculate.value === "0") {
      currentCalculate.value = "0";
    } else {
      currentCalculate.value += maskedIn(val);
    }
  } else if (val === ",") {
    if (currentCalculate.value.includes(",")) {
      currentCalculate.value = currentCalculate.value;
    } else if (lastIsOperand()) {
      return currentCalculate.value;
    } else {
      currentCalculate.value += maskedIn(val);
    }
  } else if (val === "/" || val === "*" || val === "-" || val === "+") {
    if (lastIsOperand()) {
      let str = currentCalculate.value.toString();
      let vars = str.slice(0, -1) + val;
      currentCalculate.value = maskedIn(vars);
    } else {
      currentCalculate.value += maskedIn(val);
    }
  } else {
    if (currentCalculate.value === "0" || currentCalculate.value === "NaN") {
      currentCalculate.value = maskedIn(val);
    } else {
      currentCalculate.value += maskedIn(val);
    }
  }
};
const scrollRight = () => {
  let scrollbar = document.getElementById("numberInput");
  scrollbar.scrollLeft = scrollbar.scrollWidth;
};
const scrolldown = () => {
  let scrollbar = document.getElementById("resultElement");
  scrollbar.scrollTop = scrollbar.scrollHeight;
};
const calculate = () => {
  if (lastIsOperand()) {
    let str = currentCalculate.value.toString();
    let vars = str.slice(0, -1);
    currentCalculate.value = vars;
    if (checkContainsOperand(currentCalculate)) {
      counted();
    } else {
      return currentCalculate;
    }
  } else {
    counted();
  }
};
const counted = () => {
  let rep = currentCalculate.value.replace("X", "*").replace(",", ".");
  let lastCalculate = currentCalculate.value;
  let currentCalt = eval(rep).toString();
  addHistroy(lastCalculate, currentCalt.replace(".", ","));
  currentCalculate.value = currentCalt.replace(".", ",");
};
const maskedIn = (val) => {
  return val.toString().replace("*", "X");
};
const lastIsOperand = () => {
  let operand = currentCalculate.value.slice(-1);
  if (
    operand === "+" ||
    operand === "-" ||
    operand === "X" ||
    operand === "/"
  ) {
    return true;
  } else {
    return false;
  }
};
const checkContainsOperand = (calculateString) => {
  const operand = ["+", "-", "X", "/"];
  let str = calculateString;
  return operand.some((v) => str.includes(v));
};
const addHistroy = (operation, result) => {
  if (checkContainsOperand(currentCalculate.value)) {
    histroryOperation.value.push({
      operation: operation,
      result: result,
    });
  }
};
const deleteHistory = () => {
  histroryOperation.value = [];
};
</script>
<template>
  <div class="w-full h-screen p-0 md:p-2">
    <div class="main-container">
      <calculate-display
        :deleteHistory="deleteHistory"
        :histroryOperation="histroryOperation"
        :currentCalculate="currentCalculate"
        :lastOperation="lastOperation"
      />
      <hr>
      <div class="mt-5 button-wrapper">
        <div class="flex justify-between w-full space-x-2 text-gray-300">
          <button-input class="btn-primary" symbols="AC" @click="allClear()" />
          <button-input class="btn-primary" symbols="DEL" @click="doDelete()" />
          <button-input class="btn-primary" symbols="%" @click="getPercent()" />
          <button-input
            class="btn-primary"
            symbols="/"
            @click="inputValue('/')"
          />
        </div>
        <div class="flex justify-between w-full space-x-2 text-white h-1/6">
          <button-input
            class="btn-primary"
            symbols="7"
            @click="inputValue(7)"
          />
          <button-input
            class="btn-primary"
            symbols="8"
            @click="inputValue(8)"
          />
          <button-input
            class="btn-primary"
            symbols="9"
            @click="inputValue(9)"
          />
          <button-input
            class="btn-primary"
            symbols="X"
            @click="inputValue('*')"
          />
        </div>
        <div class="flex justify-between w-full space-x-2 text-white h-1/6">
          <button-input
            class="btn-primary"
            symbols="4"
            @click="inputValue(4)"
          />
          <button-input
            class="btn-primary"
            symbols="5"
            @click="inputValue(5)"
          />
          <button-input
            class="btn-primary"
            symbols="6"
            @click="inputValue(6)"
          />
          <button-input
            class="btn-primary"
            symbols="-"
            @click="inputValue('-')"
          />
        </div>
        <div class="flex justify-between w-full space-x-2 text-white h-1/6">
          <button-input
            class="btn-primary"
            symbols="1"
            @click="inputValue(1)"
          />
          <button-input
            class="btn-primary"
            symbols="2"
            @click="inputValue(2)"
          />
          <button-input
            class="btn-primary"
            symbols="3"
            @click="inputValue(3)"
          />
          <button-input
            class="btn-primary"
            symbols="+"
            @click="inputValue('+')"
          />
        </div>
        <div class="flex justify-between w-full space-x-2 text-white h-1/6">
          <button-input
            class="btn-primary"
            symbols="00"
            @click="inputValue('00')"
          />
          <button-input
            class="btn-primary"
            symbols="0"
            @click="inputValue('0')"
          />
          <button-input
            class="btn-primary"
            symbols=","
            @click="inputValue(',')"
          />
          <button-input
            class="btn-secondary"
            symbols="="
            @click="calculate()"
          />
        </div>
        <p class="text-xl text-center text-gray-400 pt-7">Powered by <a href="https://t.me/fullstackmaster007">Og'abek</a></p>
      </div>
    </div>
  </div>
</template>

<style scoped lang="postcss">
.main-container {
  @apply m-auto w-full md:w-2/4 px-2 lg:w-1/4 rounded-none md:rounded-md ring-1 ring-blue-900 bg-white h-full flex flex-col;
}
.button-wrapper {
  @apply flex-1 py-2 flex flex-col space-y-2 text-3xl font-bold select-none;
}
.btn-primary {
  @apply bg-white border text-gray-600 rounded-2xl;
}

.btn-secondary {
  @apply bg-orange-500 text-white;
}
</style>
