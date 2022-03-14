<template>
  <section>
    <div class="AddPhotoSVG"
         @click.self="$emit('click', $event)">
      <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M14.75 8H1.25M8 1.25V14.75V1.25Z" stroke="#9296AD" stroke-width="1.5" stroke-linecap="round"
              stroke-linejoin="round" fill="currentColor"/>
      </svg>
    </div>
  </section>
</template>

<script>
export default {
  name: "AddPhotoSVG",
}
</script>

<style lang="scss" scoped>
.AddPhotoSVG {
  display: flex;
  justify-content: center;
  -webkit-align-items: center;
  -ms-flex-align: center;
  align-items: center;
  border: 1px solid #dbdbdb;
  border-radius: 50%;
  height: 48px;
  margin: auto;
  width: 48px;

  &:hover {
    cursor: pointer;
  }
}
</style>