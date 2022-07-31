<template>
  <div id="app">
    <h1>vue3-img-input</h1>
    <div class="img-container">
      <v-image-input
        v-model="image1"
        :max-file-size="0.002"
        @error:fileSize="logErrorFileSize"
        @error:fileType="logErrorFileType"
      />
    </div>
    <h2>Removable</h2>
    <div class="img-container">
      <v-image-input v-model="image2" removable />
    </div>
  </div>
</template>

<script>
import VImageInput from "@/components/VImageInput";

const ex1 = require("@/assets/ex1.jpg");
export default {
  name: "HelloWorld",
  components: { VImageInput },
  data() {
    return {
      image1: ex1,
      image2: null,
    };
  },
  methods: {
    logErrorFileSize() {
      console.log("error:fileSize");
    },
    logErrorFileType() {
      console.log("error:fileType");
    },
  },
};
</script>

<style lang="scss">
@import "@/styles/override.scss";
@import "@/styles/style.scss";
.img-container {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  width: 240px;
  height: 240px;
}
</style>
