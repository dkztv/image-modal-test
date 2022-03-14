<template>
  <AttachFilesSVG
      @click="openFileAttachModal = !openFileAttachModal"/>
  <MultipleImagesPreview
      v-if="openFileAttachModal"
      @closeuploadmodal="openFileAttachModal = !openFileAttachModal"
  />
</template>

<script>
import MultipleImagesPreview from '@/components/MultipleImagesPreview.vue';
import AttachFilesSVG from "@/components/svg/AttachFilesSVG.vue";

export default {
  name: 'App',
  components: {
    MultipleImagesPreview,
    AttachFilesSVG
  },
  data() {
    return {
      openFileAttachModal: false
    }
  },
}
</script>

<style>
#app {
  font-family: 'Source Sans Pro', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
