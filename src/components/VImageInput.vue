<template>
  <div
    class="image-input"
    @dragleave="onDragLeave"
    @dragover.prevent="onDragOver"
    @drop.prevent="handleFileDrop"
  >
    <div v-if="!modelValue" class="wrapper">
      <label
        :for="uid"
        class="input-label empty-layout"
        :class="{ 'is-drag-over': isDragOver }"
      >
        <slot name="empty-layout">
          <svg
            class="input-icon"
            id="Layer_1"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            x="0px"
            y="0px"
            fill="currentColor"
            viewBox="0 0 460.002 460.002"
            xml:space="preserve"
          >
            <path
              d="M427.137,0H32.865C14.743,0,0,14.743,0,32.865v408.543c0,10.253,8.341,18.594,18.594,18.594h408.543
            c18.122,0,32.865-14.743,32.865-32.865V32.865C460.002,14.743,445.259,0,427.137,0z M139.001,56.001c39.149,0,71,31.851,71,71
            s-31.851,71-71,71c-39.149,0-71-31.851-71-71C68.001,87.852,99.852,56.001,139.001,56.001z M405.24,393.215
            c-2.634,4.801-7.675,7.786-13.151,7.786H67.913c-5.477,0-10.518-2.984-13.151-7.786c-2.634-4.802-2.442-10.657,0.501-15.275
            l77.092-120.984c2.754-4.322,7.524-6.939,12.65-6.939s9.896,2.617,12.65,6.939l37.029,58.111l72.346-113.536
            c2.754-4.323,7.524-6.939,12.65-6.939c5.125,0,9.896,2.617,12.65,6.939L404.739,377.94
            C407.682,382.559,407.874,388.414,405.24,393.215z"
            />
          </svg>
        </slot>
      </label>
    </div>
    <div v-else class="wrapper">
      <label
        :for="uid"
        :class="{ 'is-drag-over': isDragOver }"
        class="input-label hover-label pointer"
      >
        <img class="selected-image" alt="" :src="imgSrc" />
      </label>
      <div @click.prevent="removeImage" v-if="removable" class="remove-icon">
        <slot name="remove-icon">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 320 512"
            fill="currentColor"
          >
            <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
            <path
              d="M310.6 361.4c12.5 12.5 12.5 32.75 0 45.25C304.4 412.9 296.2 416 288 416s-16.38-3.125-22.62-9.375L160 301.3L54.63 406.6C48.38 412.9 40.19 416 32 416S15.63 412.9 9.375 406.6c-12.5-12.5-12.5-32.75 0-45.25l105.4-105.4L9.375 150.6c-12.5-12.5-12.5-32.75 0-45.25s32.75-12.5 45.25 0L160 210.8l105.4-105.4c12.5-12.5 32.75-12.5 45.25 0s12.5 32.75 0 45.25l-105.4 105.4L310.6 361.4z"
            />
          </svg>
        </slot>
      </div>
    </div>
    <input
      type="file"
      class="input"
      :id="uid"
      :value="!modelValue ? modelValue : null"
      @change="handleFileInput"
      :accept="acceptedTypesString"
    />
  </div>
</template>

<script>
let uid = 0;
export default {
  name: "VImageInput",
  emits: ["update:modelValue", "error:fileSize", "error:fileType"],
  props: {
    modelValue: {
      type: [Object, String, null],
    },
    removable: {
      type: Boolean,
      required: false,
      default: false,
    },
    maxFileSize: {
      type: Number,
      required: false,
      default: 10,
    },
    acceptedTypes: {
      type: Array,
      required: false,
      default: () => {
        return ["png", "svg", "jpg", "jpeg"];
      },
    },
  },
  data() {
    uid += 1;
    return {
      uid: `image-input${uid}`,
      isDragOver: false,
      imgSrc: null,
    };
  },
  computed: {
    acceptedTypesString() {
      return this.acceptedTypes
        .map((type) => {
          return `.${type}`;
        })
        .join(", ");
    },
  },
  watch: {
    modelValue: {
      immediate: true,
      handler(newValue) {
        if (!newValue || typeof this.modelValue === "string") {
          this.imgSrc = newValue;
        } else {
          const reader = new FileReader();
          reader.onload = (e) => {
            this.imgSrc = e.target.result;
          };
          reader.readAsDataURL(newValue);
        }
      },
    },
  },
  methods: {
    isValidFileType(file) {
      const parts = file.name.toLowerCase().split(".");
      const fileType = parts[parts.length - 1];
      const acceptedTypes = this.acceptedTypes.map((t) => {
        return t.toLowerCase();
      });
      return acceptedTypes.includes(fileType);
    },
    isValidFileSize(file) {
      const fileSize = file.size / 1024 / 1024; // mb
      return this.maxFileSize > fileSize;
    },
    handleFileDrop(e) {
      const file = e.dataTransfer.files[0];
      if (!file) return;
      if (!this.isValidFileType(file)) {
        return this.$emit("error:fileType");
      }
      if (!this.isValidFileSize(file)) {
        return this.$emit("error:fileSize");
      }
      this.$emit("update:modelValue", file);
      this.isDragOver = false;
    },
    handleFileInput(e) {
      const file = e.target.files[0];
      if (!file) return;
      if (!this.isValidFileType(file)) {
        return this.$emit("error:fileType");
      }
      if (!this.isValidFileSize(file)) {
        return this.$emit("error:fileSize");
      }
      this.$emit("update:modelValue", file);
    },
    removeImage() {
      this.$emit("update:modelValue", null);
    },
    onDragOver() {
      this.isDragOver = true;
    },
    onDragLeave() {
      this.isDragOver = false;
    },
  },
};
</script>

<style scoped lang="scss">
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
</style>
