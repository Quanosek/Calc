<script setup lang="ts">
const props = defineProps({
  action: Function,
  name: {
    type: String,
    required: true,
  },
  alt: Array,
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
  <button :id="props.name" class="win-btn" @click="handleClick">
    <p>{{ props.name }}</p>
  </button>
</template>

<style lang="scss" scoped>
button {
  aspect-ratio: 1/1;

  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;

  background-color: #2c2c2c;
  transition: all 0.1s ease-in-out;
  cursor: pointer;

  &:hover,
  &:focus {
    color: #d5d5d5;
    background-color: #131313;
  }

  &.relative p {
    position: relative;
    bottom: 0.5rem;
  }
}
</style>
