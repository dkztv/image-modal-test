<template>
  <section class="FileUpload">
    <label v-show="previewFileSrc === ''"
           class="FileUpload__container">
      <img class="FileUpload__container_upload-image"
           src="@/assets/plus_scale.svg">
      <p class="FileUpload__container_upload-title">Выберите фото</p>
      <input type="file"
             id="multiple-image-upload-files"
             class="FileUpload__container_upload-input"
             accept="image/*"
             multiple
             @change="$emit('change', $event)"/>
    </label>
    <img v-if="previewFileSrc !== ''"
         class="FileUpload__image-container"
         :src="previewFileSrc"/>
  </section>
</template>

<script>
export default {
  name: "FileUpload",
  props: {
    previewFileSrc: String
  },
}
</script>

<style lang="scss" scoped>
.FileUpload {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 300px;
  height: 300px;
  margin: auto;
  border-radius: 10px;
  color: #000;

  &__image-container {
    object-fit: contain;
    background: rgba(26, 26, 26, 0.8);
    border-radius: 8px;
    height: 100%;
    width: 100%;
    display: flex;
  }

  &__container {
    outline: 2px dashed #5d5d5d;
    outline-offset: -12px;
    position: relative;
    height: 100%;
    width: 100%;
    display: flex;
    align-items: center;
    flex-direction: column;

    &:hover {
      cursor: pointer;
    }

    &_upload-title {
      position: absolute;
      bottom: 30px;
      margin: 0;
    }

    &_upload-image {
      width: 50%;
      margin: auto;
      user-select: none;
    }

    &_upload-input {
      width: 0.1px;
      height: 0.1px;
      opacity: 0;
      position: absolute;
      z-index: -10;
    }
  }
}
</style>