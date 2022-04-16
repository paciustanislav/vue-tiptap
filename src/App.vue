<template>
  <div id="app">
    <tip-tap v-model="tiptap" :limit="500"/>
  </div>
</template>

<script>

import TipTap from './components/tip-tap.vue'

export default {

  name: 'App',

  components: {
    TipTap
  },

  data: () => ({
    tiptap: 'Java <mark>Script</mark>'
  })

}

</script>