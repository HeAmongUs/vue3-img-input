# vue3-img-input

# Demo
https://vue3-img-input.vercel.app/

## Getting Started

```
npm i vue3-img-input
```

```js
import VImageInput from 'vue3-img-input'
app.component('v-image-input', VImageInput)
```

## props
v-model is required
```js
modelValue: {
    type: [Object, String, null],
}
removable: {
    type: Boolean,
    required: false,
    default: false,
}
acceptedTypes: {
    type: Array,
    required: false,
    default: ["png", "svg", "jpg", "jpeg"],
}
```

## slots 
1. slot name="empty-layout" default =>:

![empty](src/assets/empty.png)

2. slot name="remove-icon" default =>:

![img_2.png](src/assets/img_2.png)

## Usage example

```vue
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
export default {
  name: "HelloWorld",
  components: { VImageInput },
  data() {
    return {
      image1: ex1,
      image2: null,
    };
  },
};
</script>
```

