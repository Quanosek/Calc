<script setup lang="ts">
import Button from "./components/button.vue";
import { ref } from "vue";

const result = ref("");

const addToDisplay = (value: string) => {
  return (result.value += value);
};

const clearDisplay = () => {
  return (result.value = "");
};

const calculateResult = () => {
  try {
    result.value = eval(result.value.replace(/×/g, "*").replace(/÷/g, "/"));
  } catch (err) {
    result.value = "Error";
  }
};
</script>

<template>
  <main>
    <div class="frame">
      <div class="result" type="text">
        <p>{{ result }}</p>
      </div>

      <div class="grid">
        <Button :action="clearDisplay" name="AC" :alt="[`Escape`, `Delete`]" />
        <Button name="( )" :alt="[`(`, `)`, `[`, `]`]" />
        <Button :action="() => addToDisplay('%')" name="%" />
        <Button :action="() => addToDisplay('÷')" name="÷" :alt="[`/`]" />

        <Button :action="() => addToDisplay('7')" name="7" />
        <Button :action="() => addToDisplay('8')" name="8" />
        <Button :action="() => addToDisplay('9')" name="9" />
        <Button :action="() => addToDisplay('×')" name="×" :alt="[`*`]" />

        <Button :action="() => addToDisplay('4')" name="4" />
        <Button :action="() => addToDisplay('5')" name="5" />
        <Button :action="() => addToDisplay('6')" name="6" />
        <Button :action="() => addToDisplay('-')" name="-" />

        <Button :action="() => addToDisplay('1')" name="1" />
        <Button :action="() => addToDisplay('2')" name="2" />
        <Button :action="() => addToDisplay('3')" name="3" />
        <Button :action="() => addToDisplay('+')" name="+" />

        <Button :action="() => addToDisplay('0')" name="0" />
        <Button :action="() => addToDisplay('.')" name="," :alt="[`.`]" />
        <Button name="←" :alt="[`Backspace`]" />
        <Button :action="calculateResult" name="=" :alt="[`Enter`]" />
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
}
.grid {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 1rem;
}
</style>
