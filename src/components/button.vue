<script setup lang="ts">
const props = defineProps({
  action: {
    type: Function,
  },
  name: {
    type: String,
    required: true,
  },
  alt: {
    type: Array,
  },
});

function handleClick() {
  if (props.action) props.action();

  const button = document.getElementById(props.name) as HTMLButtonElement;
  document.addEventListener("keyup", () => button.blur());
  button.addEventListener("mouseleave", () => button.blur());
}

document.addEventListener("keydown", (e) => {
  if (e.key !== "F5") e.preventDefault();

  function selectedButton() {
    handleClick();

    const button = document.getElementById(props.name) as HTMLButtonElement;
    button.focus();
  }

  if (e.key === props.name) selectedButton();
  else if (props.alt?.includes(e.key)) selectedButton();
});
</script>

<template>
  <button :id="props.name" @click="handleClick">{{ props.name }}</button>
</template>

<style lang="scss" scoped>
button {
  aspect-ratio: 1/1;

  display: flex;
  justify-content: center;
  align-items: center;

  font-size: 1.5rem;
  font-weight: bold;
  color: #131313;
  background-color: #d5d5d5;
  border-radius: 30px;
  transition: all 0.1s ease-in-out;
  cursor: pointer;

  &:hover,
  &:focus {
    color: #d5d5d5;
    background-color: #131313;
  }
}
</style>
