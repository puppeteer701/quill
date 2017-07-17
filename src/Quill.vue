<template>
  <div class="quill-editor">
    <slot></slot>
  </div>
</template>

<script>
import Quill from 'quill'

export default {
  props: {
    // The form value
    value: {
      required: true
    },
    options: {
      type: Object,
      default: () => ({})
    },
    autofocus: Boolean
  },

  data () {
    return {
      focused: this.autofocus,
      editor: null,
      changedContent: null
    }
  },

  mounted () {
    this.editor = new Quill(this.$el, this.options)
    this.editor.on('text-change', (delta, source) => {
      console.log(delta, source)
      this.$emit('input', this.options.output !== 'delta' ? this.editor.root.innerHTML : this.editor.getContents())
    })
  },

  watch: {
    focused (val) {
      this.editor[val ? 'focus' : 'blur']()
    }
  }

}
</script>

<style lang="styl">
@import '~quill/assets/core'
</style>
