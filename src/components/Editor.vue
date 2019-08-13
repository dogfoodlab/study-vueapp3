<template>
  <div ref="editor"></div>
</template>

<script>
import ace from 'ace-builds'

export default {
  props: {
    width: String,
    height: String,
    context: Object
  },
  watch: {
    'context.input': {
      handler (val, oldVal) {
        // console.log('setValue')
        // console.log(val.value, this.editor.getValue())

        if (!val.value && !this.editor.getValue()) {
          this.$emit('change', this.editor.getValue())
          return
        }
        if (val.value && this.editor.getValue()) {
          // console.log('lock')
          this.lock = true
        }
        this.editor.setValue(val.value, -1)
      }
    }
  },
  data () {
    return {
      editor: undefined,
      lock: false
    }
  },
  mounted () {
    this.$el.style.width = this.width || '100px'
    this.$el.style.height = this.height || '100px'

    this.editor = ace.edit(this.$refs.editor)

    this.editor.on('change', async (e) => {
      // console.log(e)
      if (this.lock) {
        this.lock = false
        return
      }
      // console.log('ace change')
      // console.log(e)
      this.context.output.value = this.editor.getValue()
      this.$emit('change', this.editor.getValue())
    })
  }
}
</script>
