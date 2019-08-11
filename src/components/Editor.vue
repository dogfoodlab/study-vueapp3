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
    context: {
      handler (val, oldVal) {
        // console.log('setValue')
        if (!val.value && !this.editor.getValue()) {
          this.$emit('change', this.editor.getValue())
          return
        }
        if (this.editor.getValue()) {
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

    const element = this.$refs.editor
    this.editor = ace.edit(element)

    this.editor.on('change', async (e) => {
      if (this.lock) {
        this.lock = false
        return
      }
      // console.log('ace change')
      // console.log(e)
      this.$emit('change', this.editor.getValue())
    })
  }
}
</script>
