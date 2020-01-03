# vue-lazy-youtube-video

![Vue.js logo plus YouTube logo](./assets/img.jpg)

- [vue-lazy-youtube-video](#vue-lazy-youtube-video)
  - [Features](#features)
  - [Installation](#installation)
    - [Via NPM](#via-npm)
    - [Via Yarn](#via-yarn)
    - [Directly in browser](#directly-in-browser)
  - [Initialization](#initialization)
    - [As a global component](#as-a-global-component)
    - [As a local component](#as-a-local-component)
    - [SSR](#ssr)
    - [As a plugin](#as-a-plugin)
  - [Usage](#usage)
  - [Demo](#demo)
  - [API](#api)
    - [Properties](#properties)
    - [Slots](#slots)
    - [Events](#events)
  - [Tests](#tests)
    - [Unit](#unit)
  - [Development](#development)
  - [Build](#build)
  - [Powered by](#powered-by)
  - [Inspiration](#inspiration)
  - [License](#license)

## Features

- reduces initial load size by ~1.1MB per video;
- built with a11y in mind – fully accessible via keyboard and to screen readers;
- `.webp` thumbnail format for modern browsers that support it, with `.jpg` fallback for browsers that don't;
- fully customizable through `props` and `slots`.

## Installation

### Via NPM

```bash
$ npm install vue-lazy-youtube-video --save
```

### Via Yarn

```bash
$ yarn add vue-lazy-youtube-video
```

### Directly in browser

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script src="https://unpkg.com/vue-lazy-youtube-video@1.x"></script>
<script>
  // as a plugin
  Vue.use(VueLazyYoutubeVideo.Plugin)
  // as a component
  Vue.component('LazyYoutubeVideo', VueLazyYoutubeVideo.default)
</script>
```

## Initialization

### As a global component

> ⚠️ It must be called before `new Vue()`.

```js
import Vue from 'vue'
import LazyYoutubeVideo from 'vue-lazy-youtube-video'

Vue.component('LazyYoutubeVideo', LazyYoutubeVideo)
```

### As a local component

```js
import LazyYoutubeVideo from 'vue-lazy-youtube-video'

export default {
  name: 'YourAwesomeComponent',
  components: {
    LazyYoutubeVideo,
  },
}
```

### SSR

There is special bundle for SSR users:

```js
import Vue from 'vue'
import LazyYoutubeVideo, {
  Plugin,
} from 'vue-lazy-youtube-video/dist/vue-lazy-youtube-video.ssr.common'
// have to import ejected style directly, see this issue https://github.com/vuejs/rollup-plugin-vue/issues/266
import 'vue-lazy-youtube-video/dist/style.css'

// as a global component
Vue.component('LazyYoutubeVideo', LazyYoutubeVideo)

// or as a plugin
Vue.use(Plugin)

// or as a local component
export default {
  // ...
  components: {
    LazyYoutubeVideo,
  },
  // ...
}
```

### As a plugin

> ⚠️ It must be called before `new Vue()`.

```js
import Vue from 'vue'
import { Plugin } from 'vue-lazy-youtube-video'

Vue.use(Plugin)
```

## Usage

```vue
<template>
  <LazyYoutubeVideo url="https://www.youtube.com/watch?v=4JS70KB9GS0" />
</template>
```

## Demo

[![Edit vue-lazy-youtube-video](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/x7nrwxq6qo)

## API

### Properties

> 🚨 Value of the `url` property must be a **DIRECT** link to the video (e.g. https://www.youtube.com/watch?v=4JS70KB9GS0), but not the **EMBED** link (e.g. https://www.youtube.com/embed/4JS70KB9GS0). Component itself will generate appropriate `src` attribute for the `<iframe />` element.

The list of available `props` (with their types, default values and descriptions) is listed below:

| Property           | Required | Type                           | Default           | Description                                                                                                                                                                                                                                    |
| ------------------ | -------- | ------------------------------ | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `url`              | `true`   | `string`                       |                   | Video URL. Supported URLs are listed [here](https://gist.github.com/rodrigoborgesdeoliveira/987683cfbfcc8d800192da1e73adc486).                                                                                                                                                                             |
| `query`            | `false`  | `string`                       | `?autoplay=1`     | [Query string](https://en.wikipedia.org/wiki/Query_string) which will be appended to the generated embed URL                                                                                                                                   |
| `alt`              | `false`  | `string`                       | `Video thumbnail` | Value of the `alt` attribute of the thumbnail `<img />` element                                                                                                                                                                                |
| `buttonLabel`      | `false`  | `string`                       | `Play video`      | Value of the `aria-label` attribute of the play `<button></button>` element. Improves a11y.                                                                                                                                                    |
| `aspectRatio`      | `false`  | `string`                       | `16:9`            | Aspect ratio of the video. This prop helps to save proportions of the video on different container sizes. Should match the `number:number` pattern                                                                                             |
| `previewImageSize` | `false`  | `string`                       | `maxresdefault`   | Size of the thumbnail, generated by YouTube. Available variants: `mqdefault`, `sddefault`, `hqdefault`, `maxresdefault`. [More info](https://stackoverflow.com/questions/2068344/how-do-i-get-a-youtube-video-thumbnail-from-the-youtube-api). |
| `thumbnail`        | `false`  | `{ webp: string, jpg: string}` |                   | Custom thumbnail object, which should contain two keys: `webp` and `jpg`. Value of the key is the path to the custom thumbnail image                                                                                                           |
| `noCookie`         | `false`  | `boolean`                      | `false`           | Whether or not to enable privacy-enhanced mode. If `true` – component will insert `-nocookie` part into the generated embed link                                                                                                               |
| `iframeAttributes` | `false`  | `object`                       |                   | Custom attributes you want to assign to the `<iframe />`                                                                                                                                                                                       |
| `autoplay`         | `false`  | `boolean`                      | `false`           | Whether or not to play video as soon as possible                                                                                                                                                                                               |

### Slots

Component provides you with some `slots` that you can use to fit your needs.
The list of available slots are listed below:

| Slot     | Description                                                         |
| -------- | ------------------------------------------------------------------- |
| `button` | Slot gives you an ability to provide custom play button             |
| `icon`   | Slot gives you an ability to provide custom icon of the play button |

### Events

| Name        | Type         | Usage                                                                     |
| ----------- | ------------ | ------------------------------------------------------------------------- |
| videoLoaded | `() => void` | The event that is triggered when the `<iframe>` is inserted into the DOM. |

## Tests

### Unit

[`Jest`](https://jestjs.io) and [`VueTestUtils`](https://vue-test-utils.vuejs.org) is used for unit tests.

You can run unit tests by running the next command:

```bash
npm run test:unit
```

## Development

1. Clone this repository
2. Install dependencies using `yarn install` or `npm install`
3. Start development server using `npm run dev` script

## Build

To build the production ready bundle simply run `npm run build`:

After the successful build the following files will be generated in the `dist` folder:

```
├── vue-lazy-youtube-video.common.js
├── vue-lazy-youtube-video.esm.js
├── vue-lazy-youtube-video.js
├── vue-lazy-youtube-video.min.js
├── vue-lazy-youtube-video.ssr.common.js
```

## Powered by

- `Rollup` (and plugins)
- `Babel`
- `Jest`
- `Vue Test Utils`
- `TypeScript`

## Inspiration

Inspired by [Vadim Makeev](https://pepelsbey.net). Vadim created a comprehensive tutorial in which he shows how to lazyload YouTube videos properly.

## License

[MIT](http://opensource.org/licenses/MIT)
