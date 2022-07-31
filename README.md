# vue3-img-input

# Demo and docs
https://vue3-img-input-exmaples.vercel.app/
https://vue3-img-input-docs.vercel.app/guide/

# Getting Started
## Installation
Install the component using the preferred package manager
```bash
    npm i vue3-img-input
```
Then import and register component

> Note: `css` file is imported separately
### Global
In the main.js file
```js 
    // main.js
    import VImageInput from 'vue3-img-input'
    app.component('v-image-input', VImageInput)
```

## Local
In the component.vue file
```js
    // Coomponent.vue <script>
    import VImageInput from "@/components/VImageInput";
    
    export default {
        components: {
            ...
            VImageInput,
            ...
        }
    }
```
## props
v-model is required
```js
    modelValue: {
        type: [Object, String, null],
    }
```
Removable - show remove icon
```js
    removable: {
        type: Boolean, 
        required: false,
        default: false,
    }
```
File size in MByte

if the uploaded file large then maxFileSize +> emit `error:fileSize'
```js
    maxFileSize: {
        type: Number,
        required: false,
        default: 10,
    }
```
AcceptedTypes: `Array<String>`

if the uploaded file not includes in +> emit `error:fileSize'
```js
    acceptedTypes: {
        type: Array,
        required: false,
        default: () => {
            return ["png", "svg", "jpg", "jpeg"];
        }
    }
```

## events 
### general 
v-model update
```js
    // update:modelValue
    <v-image-input v-model="image1" />
```
### errors:
File type error
emits when the type of uploaded file not includes in `acceptedTypes:Array<string>` **props**
```vue
    <v-image-input @error:fileType="errorHandler" />
```

File size error
emits when the size of uploaded file is larger then `maxFileSize:Number` **props**
```vue
    <v-image-input @error:fileSize="errorHandler" />
```

## slot
### #empty-layout
change this for update empty-layout image|text

### #remove-icon
change this for update remove-icon

## SCSS Variables
```scss
$v-image-input-color: #2c3e50 !default;
$v-image-input-color-hover: #4689cd !default;
$v-image-input-background: #fff !default;
$v-image-input-background-hover: #eee !default;

$v-image-input-width: 100% !default;
$v-image-input-height: 100% !default;

$v-image-input-empty-layout-padding: 20px !default;
$v-image-input-empty-layout-border: 1px solid #2c3e50 !default;
$v-image-input-border-radius: 20px !default;
$v-image-input-object-fit: fill !default;

$v-image-input-rm-icon-color: #fff !default;
$v-image-input-rm-icon-size: 24px !default;
$v-image-input-rm-icon-indent: -10px !default;
$v-image-input-rm-icon-position: absolute !default;
$v-image-input-rm-icon-border-radius: 50% !default;
$v-image-input-rm-icon-transition: 0.3s !default;
$v-image-input-rm-icon-color-hover: #000 !default;
$v-image-input-rm-icon-background: #2c3e50 !default;
$v-image-input-rm-icon-background-hover: #4689cd !default;
```

## Usage example

```vue
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

```

