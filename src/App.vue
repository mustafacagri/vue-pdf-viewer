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