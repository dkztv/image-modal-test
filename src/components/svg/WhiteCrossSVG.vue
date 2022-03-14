<template>
  <div class="WhiteCrossSVG"
       @click.self="$emit('click', $event)">
    <div class="WhiteCrossSVG__inside">
      <svg aria-label="Delete" class="_8-yf5 " color="#ffffff" fill="#ffffff" height="12" role="img" viewBox="0 0 24 24"
           width="12">
        <line fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" x1="21"
              x2="3" y1="3" y2="21"></line>
        <line fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" x1="21"
              x2="3" y1="21" y2="3"></line>
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  name: "WhiteCrossSVG"
}
</script>

<style lang="scss" scoped>
.WhiteCrossSVG {
  position: absolute;
  right: 1px;
  top: 1px;
  width: 22px;
  height: 22px;

  &:hover {
    cursor: pointer;
  }

  &__inside {
    width: 18px;
    height: 18px;
    padding: 2px;
    background: rgba(26,26,26,.8);
    border-radius: 50%;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    cursor: pointer;
    opacity: 1;

    &:hover {
      opacity: 0.5;
    }

    & svg {
      margin: auto;
    }
  }
}
</style>