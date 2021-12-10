# A NativeScript-Vue component for "Slide to unlock"

Slide to unlock in iOS style with animations made with NativeScript-Vue. Works on iOS and Android.

[![npm](https://img.shields.io/npm/v/@nativescript-community/ui-vue-unlock-slider.svg)](https://www.npmjs.com/package/@nativescript-community/ui-vue-unlock-slider)

<img src="https://raw.githubusercontent.com/nativescript-community/ui-vue-slide-to-unlock/master/demo.gif" width="300">

## Installation

```
ns plugin add @nativescript-community/ui-vue-unlock-slider
```

## Usage
```diff
// app.js
import Vue from 'nativescript-vue'
...
+ import UnlockSlider from '@nativescript-community/ui-vue-unlock-slider'
+ Vue.use(UnlockSlider)
```

```xml
<UnlockSlider ref="unlockSlider" @change="sliderChange" />
```

```js
export default {
  data() {
    return {
      slidePercent: 0
    }
  },
  methods: {
    sliderChange(percent) {
      this.slidePercent = percent;
    },
    reset() {
      this.$refs.unlockSlider.reset();
    }
  }
}
```

## Properties
| Name                     | Type   | Default value   |
| ------------------------ | ------ | --------------- |
| height                   | Number | 70              |
| radius                   | Number | 70              |
| containerBackgroundColor | String | lightgray       |
| buttonText               | String | →               |
| buttonTextSize           | Number | 20              |
| buttonTextColor          | String | black           |
| buttonTextFontWeight     | String | normal          |
| buttonBackgroundColor    | String | white           |
| infoText                 | String | Slide to unlock |
| infoTextSize             | Number | 16              |
| infoTextColor            | String | black           |

## Events
| Name   | Type   | Value   |
| -------| ------ | ------- |
| change | Number | 0.00-1.00 |
