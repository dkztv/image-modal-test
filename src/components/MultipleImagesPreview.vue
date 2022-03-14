<template>
  <section class="MultipleImagesPreview">
    <div class="MultipleImagesPreview__outside"
         @click="$emit('closeuploadmodal')">
      <CloseSVG/>
    </div>
    <div class="MultipleImagesPreview__inside">
      <h2>Добавление изображений</h2>
      <hr/>

      <FileUpload :previewFileSrc="previewFileSrc"
                  @change.self="handleFilesUpload($event)"/>

      <div v-if="files.length !== 0"
           class="MultipleImagesPreview__inside_uploaded">
        <div ref="uploadedImages"
             class="MultipleImagesPreview__inside_uploaded--list">
          <div v-for="(file, key) in files"
               :key="key"
               :class="{'MultipleImagesPreview__inside_uploaded--list-elem-selected': selectedFile === file}"
               draggable="true"
               class="MultipleImagesPreview__inside_uploaded--list-elem"
               @click="selectedFile = file; previewFileSrc = file.src"
               @dragstart="onDragStart"
               @dragend="onDragEnd"
               @dragover="onDragOver($event, file)">
            <img :id="'image-'+parseInt(key)"
                 :name="file.name"/>
            <WhiteCrossSVG
                v-if="selectedFile === file"
                @click="removeFile(key)"/>
          </div>
          <AddPhotoSVG class="MultipleImagesPreview__inside_uploaded--list-elem-plus"
                       @click="addFiles"/>
        </div>
      </div>

      <div class="MultipleImagesPreview__inside_button-cont">
        <button
            @click="submitFiles()"
            :disabled="emptyFiles"
        >Отправить</button>
      </div>
      <div v-if="sendingText">
        <p :class="{'MultipleImagesPreview__error': sendingStatus === false}">{{sendingText}}</p>
      </div>
    </div>
  </section>
</template>

<script>
import axios from 'axios';
import config from "@/config";

import FileUpload from '@/components/FileUpload.vue';
import CloseSVG from '@/components/svg/CloseSVG.vue';
import AddPhotoSVG from "@/components/svg/AddPhotoSVG";
import WhiteCrossSVG from "@/components/svg/WhiteCrossSVG";

export default {
  name: 'MultipleImagesPreview',
  components: {
    WhiteCrossSVG,
    AddPhotoSVG,
    FileUpload,
    CloseSVG,
  },
  data() {
    return {
      files: [],
      selectedFile: null,
      previewFileSrc: '',
      sendingStatus: null,
      sendingText: ''
    }
  },
  computed: {
    emptyFiles() {
      return this.files.length === 0
    }
  },
  watch: {
    selectedFile(newValue) {
      if (!newValue)
        this.previewFileSrc = ''
    }
  },
  methods: {
    addFiles() {
      document.getElementById('multiple-image-upload-files').click();
    },

    removeFile(key) {
      const fileToDelete = this.files[key] || null
      this.files.splice(key, 1)

      setTimeout(() => {
        if (this.selectedFile === fileToDelete) {
          this.selectedFile = this.files[this.files.length - 1]
        }
      }, 0)
      this.getImagePreviews();
    },

    submitFiles() {
      let formData = new FormData();

      for( let i = 0; i < this.files.length; i++ ){
        let file = this.files[i];

        formData.append('files[' + i + ']', file);
      }

      axios.post(`${config.apiUrl}/upload-files`,
          formData,
          {
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          })
          .then(() => {
            this.sendingStatus = true
            this.files = []
          })
          .catch(() => {
            this.sendingStatus = false
          })
          .finally(() => {
            if (this.sendingStatus) {
              this.sendingText = `Изображения успешно отправлены! Status: ${this.sendingStatus}`
            } else {
              this.sendingText = `Изображения не удалось отправить! Status: ${this.sendingStatus}`
            }
            setTimeout(() => {
              this.sendingStatus = null
              this.sendingText = ''
            }, 3000)
      });
    },

    handleFilesUpload(event) {
      let uploadedFiles = event.target.files;

      for( let i = 0; i < uploadedFiles.length; i++ ){
        if (this.files.length === 0)
          this.selectedFile = uploadedFiles[i]
        this.files.push( uploadedFiles[i] );
      }

      this.getImagePreviews();
    },

    getImagePreviews() {
      for( let i = 0; i < this.files.length; i++ ){
        if ( /\.(jpe?g|png|gif)$/i.test( this.files[i].name ) ) {
          let reader = new FileReader();

          reader.addEventListener("load", function() {
            const imageElem = document.getElementById('image-'+parseInt(i))
            if(imageElem) {
              imageElem.src = reader.result
            }
            this.files[i].src = reader.result
            if (this.files[i] === this.selectedFile) {
              this.previewFileSrc = reader.result
            }
          }.bind(this), false);

          reader.readAsDataURL( this.files[i] );
        }
      }
    },

    onDragStart(event) {
      event.target.parentElement.classList.add(`selected`);
    },

    onDragEnd(event) {
      event.target.parentElement.classList.remove(`selected`);
    },

    onDragOver(event, file) {
      event.preventDefault();

      const activeElement = this.$refs.uploadedImages.querySelector(`.selected`);
      const currentElement = event.target.parentElement;
      const isMovable = activeElement !== currentElement &&
          currentElement.classList.contains(`MultipleImagesPreview__inside_uploaded--list-elem`);
      if (!isMovable) {
        return;
      }

      const nextElement = this.getNextElement(event.clientX, currentElement);

      if (
          nextElement &&
          activeElement === nextElement.previousElementSibling ||
          activeElement === nextElement
      ) {
        return;
      }

      const nextFileElem = this.files.find(file => file.name === nextElement.children[0].name)
      const tempFileElem = file;
      this.files[this.files.indexOf(file)] = nextFileElem
      this.files[this.files.indexOf(nextFileElem)] = tempFileElem
      this.$refs.uploadedImages.insertBefore(activeElement, nextElement);

    },

    getNextElement(cursorPosition, currentElement) {
      const currentElementCoord = currentElement.getBoundingClientRect();
      const currentElementCenter = currentElementCoord.x + currentElementCoord.width / 2;

      const nextElement = (cursorPosition < currentElementCenter) ?
          currentElement :
          currentElement.nextElementSibling;

      return nextElement;
    }
  }
}
</script>

<style lang="scss" scoped>

.MultipleImagesPreview {
  &__outside {
    position: fixed;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    width: 100%;
    height: 100%;
    opacity: 0.75;
    background-color: #000;
    z-index: 1;

  }

  &__inside {
    position: absolute;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    width: 75%;
    height: 75%;
    margin: auto;
    padding: 22px 24px 0 24px;
    background: #fcfcfc;
    box-shadow: 0 4px 8px rgba(53, 52, 66, 0.04), 0 0 2px rgba(53, 52, 66, 0.06),
    0 0 1px rgba(53, 52, 66, 0.04);
    border-radius: 4px;
    z-index: 2;

    &_uploaded {
      background: rgba(26,26,26,.8);
      border-radius: 8px;
      z-index: 3;
      width: 100%;
      margin-top: 10px;

      &--list {
        display: flex;
        padding: 5px 0 5px 5px;
        overflow: auto;

        &-elem {
          position: relative;
          width: 90px;
          height: 100px;
          display: flex;
          margin-right: 5px;
          opacity: 0.5;
          cursor: move;
          transition: background-color 0.5s;

          &-selected {
            opacity: 1;
          }

          &-plus {
            width: 55px;
            height: 100px;
            display: flex;
            margin-right: 5px;
          }

          img {
            object-fit: cover;
            overflow: hidden;
            border-radius: 10px;
            transition: none 0s ease 0s;

          }
        }
      }
    }

    &_button-cont {
      margin-top: 10px;

      & button {
        font-size: 14px;
        font-weight: 600;
        padding: 10px 15px;
        text-align: center;
        border-radius: 4px;
        color: #fff;
        background-color: rgb(0,149,246);
        border: 1px solid transparent;
        cursor: pointer;

        &:disabled {
          cursor: not-allowed;
          background: rgba(26,26,26,.5);
        }
      }
    }


  }

  &__error {
    color: #ff364e
  }
}
</style>