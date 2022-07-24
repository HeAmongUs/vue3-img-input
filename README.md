# vue3-img-input

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


