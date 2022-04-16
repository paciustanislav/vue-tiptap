<template>
  <div>

    <bubble-menu v-if="editor" :editor="editor" :tippy-options="{ duration: 100 }">
      <button @click="editor.chain().focus().toggleHighlight().run()" :class="{ 'is-active': editor.isActive('highlight') }">
        toggleHighlight
      </button>
    </bubble-menu>

    <p>
      <editor-content :editor="editor" />
    </p>
    <p>
      {{ editor.storage.characterCount.characters() }}/{{ limit }}
    </p>
    <p>
      <button @click="get_json">get json</button>
    </p>



  </div>
</template>

<script>

import { Editor, EditorContent, BubbleMenu } from '@tiptap/vue-2'
import Document from '@tiptap/extension-document'
import Paragraph from '@tiptap/extension-paragraph'
import Text from '@tiptap/extension-text'
// import Typography from '@tiptap/extension-typography'
import Highlight from '@tiptap/extension-highlight'
import CharacterCount from '@tiptap/extension-character-count'

export default {

  components: {
    EditorContent,
    BubbleMenu
  },

  props: {
    value: {
      type: String,
      default: '',
    },
    limit: {
      type: Number,
      default: null
    }
  },

  data: () => ({
    editor: null
  }),

  methods: {

    get_json () {
      // console.log( this.editor.getHTML() )
      console.log( this.editor.getJSON() )
    }

  },

  watch: {
    value ( value ) {

      // HTML
      const isSame = this.editor.getHTML() === value

      // JSON
      // const isSame = JSON.stringify(this.editor.getJSON()) === JSON.stringify(value)

      if (isSame) {
        return
      }

      this.editor.commands.setContent(value, false)
    },
  },

  mounted() {
    this.editor = new Editor({
      content: this.value,
      extensions: [
        Document,
        Paragraph,
        Text,
        Highlight,
        CharacterCount.configure({
          limit: this.limit,
        }),
      ],
      onUpdate: () => {
        // HTML
        this.$emit('input', this.editor.getHTML())

        // JSON
        // this.$emit('input', this.editor.getJSON())
      },
    })
  },

  beforeDestroy() {
    this.editor.destroy()
  }

}

</script>

<style scoped>

.is-active {
  padding: 5px;
  border: 1px solid #000;
}

</style>