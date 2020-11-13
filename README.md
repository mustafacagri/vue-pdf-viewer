# Vue Pdf Viewer

![altcenter](https://www.mustafacagri.com/wp-content/uploads/2020/11/vue-pdf-viewer.gif "Vue Pdf Viewer")


## Demo

https://vue-pdf-viewer.web.app/

## What's Happening Here?

```
<Pdf file="/docs/pdf.pdf" title="My Beautiful PDF File"></Pdf>
```

Here you specify the location of pdf file and the title of card header.

## Number / Current of PDF

At right top side, you see the number of pages.

```
<span class="right" id="PageInfo" v-if="!error">Page {{current}}/{{totalPage}}</span>
```

## Navigation

```
<button class="btn btn-sm btn-success mr-2" id="prev-page" @click="RenderPage(current-1)" :disabled="current <= 1"><i class="fas fa-arrow-circle-left mr-1"></i> Prev Page</button>
<button class="btn btn-sm btn-success" id="next-page" @click="RenderPage(current+1)" :disabled="totalPage<=current">Next Page <i class="fas fa-arrow-circle-right ml-1"></i></button>
```


## App.vue

In the main file "App.vue" we call our Pdf viewer like this:

```
<template>
  <div id="app">
    <Pdf file="/docs/pdf.pdf" title="My Beautiful PDF File"></Pdf>
  </div>
</template>

<script>
import Pdf from '@/components/Pdf.vue'
export default {
  name: 'App',
  components: {
    Pdf
  }
}
</script>
```



## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
