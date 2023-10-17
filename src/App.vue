<script setup lang="ts">
import Button from "./components/button.vue";
import { ref } from "vue";

const result = ref("");

let isError: boolean = false;
let lastOperator: boolean = false;
let currentNumber: string = "";

function Add(value: string | number) {
  if (isError) ClearButton();

  if (typeof value == "number") {
    value = value.toString();

    if (currentNumber.startsWith("0")) BackspaceButton();

    currentNumber += value;
    lastOperator = false;
  }

  result.value += value;
}

function Operator(value: string) {
  if (isError) return;
  currentNumber = "";

  if (lastOperator) {
    BackspaceButton();
  }

  Add(value);
  lastOperator = true;
}

function ClearButton() {
  result.value = "";
  isError = false;
  lastOperator = false;
  currentNumber = "";
}

function BracketsButton() {
  // if (result.value.includes("(")) {
  //   Add(")");
  // } else {
  //   Add("(");
  // }
}

function SubtractionButton() {
  if (isError || result.value.endsWith("-")) return;

  if (!currentNumber) {
    currentNumber += "-";
    Add("-");
    lastOperator = false;
  } else {
    Operator("-");
  }
}

function CommaButton() {
  // if (!currentNumber) Add(0);
  // if (currentNumber.includes(",")) return;
  // Add(",");
}

function BackspaceButton() {
  if (isError) {
    result.value = "";
    isError = false;
  }

  if (lastOperator) lastOperator = false;
  else currentNumber = currentNumber.slice(0, -1);

  result.value = result.value.slice(0, -1);
}

function GetResults() {
  if (isError) return;

  if (lastOperator) {
    if (result.value.endsWith("-")) BackspaceButton();
    BackspaceButton();
  }

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
}
</script>

<template>
  <main>
    <div class="frame">
      <div class="result" type="text">
        <p>{{ result }}</p>
      </div>

      <div class="grid">
        <Button :action="ClearButton" name="AC" :alt="[`Escape`, `Delete`]" />
        <Button
          :action="BracketsButton"
          name="( )"
          :alt="[`(`, `)`, `[`, `]`]"
        />
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
    </div>
  </main>
</template>

<style lang="scss" scoped>
.frame {
  padding: 3rem 2rem;
  width: 25rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1.5rem;
  background-color: #252525;
  border: 1.5px solid #d5d5d5;
  border-radius: 1rem;
}

.result {
  padding: 0 1.2rem;
  width: 100%;
  height: 5.5rem;
  display: flex;
  justify-content: flex-end;
  align-items: center;

  font-size: 2rem;
  color: #252525;
  background-color: #d5d5d5;
  border-radius: 0.6rem;

  overflow: hidden;
}

.grid {
  width: 100%;
  display: grid;
  font-size: 200%;
  font-weight: bold;

  grid-template-columns: repeat(4, 1fr);
  grid-gap: 1rem;

  .enlarge {
    font-size: 135%;
  }
}
</style>
