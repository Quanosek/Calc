<script setup lang="ts">
import Button from "./components/button.vue";
import { ref } from "vue";

const result = ref("");
let history: string = "";

let isError: boolean = false;
let lastOperator: boolean = false;
let currentNumber: string = "";

const Add = (value: string | number) => {
  if (isError) ClearButton();

  if (typeof value == "number") {
    value = value.toString();

    if (currentNumber.startsWith("0") && !currentNumber.startsWith("0,")) {
      BackspaceButton();
    }

    lastOperator = false;
  }

  result.value += value;
  currentNumber += value;
};

const Operator = (value: string) => {
  if (isError) return;

  if (lastOperator || result.value.endsWith(",")) BackspaceButton();
  if (currentNumber === "-") BackspaceButton(), BackspaceButton();

  Add(value);
  lastOperator = true;
  currentNumber = "";
};

const ClearButton = () => {
  result.value = "";
  history = "";

  isError = false;
  lastOperator = false;
  currentNumber = "";
};

let openBrackets: number = 0;

const BracketsButton = () => {
  if (result.value.endsWith("(")) {
    Add("(");
    openBrackets++;
  } else {
    if (!openBrackets) {
      Add("(");
      openBrackets++;
    } else {
      Add(")");
      openBrackets--;
    }
  }
};

const SubtractionButton = () => {
  if (isError || result.value.endsWith("-")) return;

  if (!currentNumber) {
    Add("-");
    lastOperator = false;
  } else {
    Operator("-");
  }
};

const CommaButton = () => {
  if (currentNumber.includes(",")) return;
  if (!currentNumber.length) Add(0);

  Add(",");
};

const BackspaceButton = () => {
  if (isError) {
    result.value = "";
    isError = false;
  }

  result.value = result.value.slice(0, -1);

  if (result.value && !currentNumber) lastOperator = true;

  if (currentNumber && !lastOperator) {
    currentNumber = currentNumber.slice(0, -1);
  } else lastOperator = false;

  if (!result.value) history = "";
};

const GetResults = () => {
  if (isError || !result.value.length) return;

  if (currentNumber === "-" || lastOperator) {
    if (result.value.endsWith("-")) BackspaceButton();
    BackspaceButton();
  }

  for (let i = 0; i < openBrackets; i++) {
    Add(")");
  }

  history = result.value;

  try {
    const input = result.value
      .replace(/\×/g, "*")
      .replace(/\÷/g, "/")
      .replace(/\,/g, ".");

    result.value = eval(input).toString().replace(/\./g, ",");
  } catch (err: any) {
    console.error(err.message);

    result.value = "Error";
    isError = true;
    lastOperator = false;
    currentNumber = "";
  }
};
</script>

<template>
  <main>
    <div class="frame">
      <div class="display" type="text">
        <p class="history">{{ history }}</p>
        <p class="result">{{ result }}</p>
      </div>

      <div class="grid">
        <Button :action="ClearButton" name="AC" :alt="[`Delete`]" />
        <Button :action="BracketsButton" name="( )" :alt="[`(`, `)`]" />
        <Button :action="() => Add('%')" name="%" />
        <Button
          :action="() => Operator('÷')"
          name="÷"
          :alt="[`/`]"
          class="enlarge"
        />

        <Button :action="() => Add(7)" name="7" />
        <Button :action="() => Add(8)" name="8" />
        <Button :action="() => Add(9)" name="9" />
        <Button
          :action="() => Operator('×')"
          name="×"
          :alt="[`*`]"
          class="enlarge"
        />

        <Button :action="() => Add(4)" name="4" />
        <Button :action="() => Add(5)" name="5" />
        <Button :action="() => Add(6)" name="6" />
        <Button :action="SubtractionButton" name="-" class="enlarge" />

        <Button :action="() => Add(1)" name="1" />
        <Button :action="() => Add(2)" name="2" />
        <Button :action="() => Add(3)" name="3" />
        <Button :action="() => Operator('+')" name="+" class="enlarge" />

        <Button :action="() => Add(0)" name="0" />
        <Button
          :action="CommaButton"
          name=","
          :alt="[`.`]"
          class="enlarge relative"
        />
        <Button
          :action="BackspaceButton"
          name="←"
          :alt="[`Backspace`]"
          class="enlarge"
        />
        <Button
          :action="GetResults"
          name="="
          :alt="[`Enter`]"
          class="enlarge"
        />
      </div>

      <p class="embossing">Made by klalo.pl</p>
    </div>
  </main>
</template>

<style lang="scss" scoped>
.frame {
  padding: 3rem 2rem;
  width: 25rem;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 0.25rem;
  color: #d5d5d5;
  background-color: #252525;
  border-radius: 1rem;
  box-shadow: 3px 10px 12px #000000;
}

.display {
  padding: 0 1.2rem;
  width: 100%;
  height: 5.5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-end;

  font-size: 2rem;
  color: #252525;
  background-color: #d5d5d5;

  overflow: hidden;

  .history {
    opacity: 70%;
    font-size: 65%;
  }
}

.grid {
  width: 100%;
  display: grid;
  font-size: 200%;
  font-weight: bold;

  grid-gap: 0.1rem;
  grid-template-columns: repeat(4, 1fr);

  .enlarge {
    font-size: 135%;
  }
}

.embossing {
  position: absolute;
  bottom: 1rem;
  font-size: 75%;
  font-weight: bold;
  text-transform: uppercase;
  color: #252525;
  text-shadow: -0.5px -0.5px 1px #131313, 0.5px 0.5px 1px #2c2c2c;
}
</style>
