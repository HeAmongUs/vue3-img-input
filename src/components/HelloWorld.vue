<template>
  <div id="app">
    <h1>vue3-img-input</h1>
    <v-image-input v-model="image1" />
    <h2>Removable</h2>
    <v-image-input v-model="image2" removable />
  </div>
</template>

<script>
import VImageInput from "@/components/VImageInput";

const ex1 = require("@/assets/ex1.jpg");
const ex2 = require("@/assets/ex2.jpg");
export default {
  name: "HelloWorld",
  components: { VImageInput },
  data() {
    return {
      image1: ex1,
      image2: ex2,
    };
  },
};
</script>

<style scoped lang="scss">
#app {
  margin: 40px;
}
</style>
