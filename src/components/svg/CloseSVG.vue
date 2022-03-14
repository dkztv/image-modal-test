<template>
  <div @click.self="$emit('click', $event)"
       class="CloseSVG">
    <svg width="40" height="40" viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M13 13L27 27M13 27L27 13" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      <circle cx="20" cy="20" r="18.75" stroke="currentColor" stroke-width="1.5"/>
    </svg>
  </div>
</template>

<script>
export default {
  name: "CloseSVG"
}
</script>

<style lang="scss" scoped>
.CloseSVG {
  position: fixed;
  top: 10px;
  right: 10px;
  width: 40px;
  height: 40px;
  color: #b7bacf;
  cursor: pointer;
  transition: opacity 0.2s;

  &:hover {
    opacity: 0.8;
  }
}
</style>