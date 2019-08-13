<template>
  <div ref="editor"></div>
</template>

<script>
// import ace from 'ace-build'
import ace from 'ace-builds/src-noconflict/ace'
import 'ace-builds/src-noconflict/theme-cobalt'
import 'ace-builds/src-noconflict/theme-solarized_light'
import 'ace-builds/src-noconflict/mode-json'
import 'ace-builds/src-noconflict/mode-yaml'

export default {
  props: {
    width: String,
    height: String,
    theme: String,
    mode: String,
    context: Object
  },
  watch: {
    'context.input': {
      handler (val, oldVal) {
        // console.log('setValue')
        // console.log(val.value, this.editor.getValue())

        const inputValue = val.value
        const editorValue = this.editor.getValue()

        if (!inputValue && !editorValue) {
          this.$emit('change', editorValue)
          return
        }

        if (inputValue && editorValue) {
          // console.log('lock')
          this.lock = true
        }

        this.editor.setValue(inputValue, 1)
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
    this.$el.style.width = this.width || '200px'
    this.$el.style.height = this.height || '100px'

    const editor = ace.edit(this.$refs.editor)
    if (this.theme) {
      editor.setTheme('ace/theme/' + this.theme)
    }

    const session = editor.getSession()
    session.setUseWorker(false)
    session.setUseSoftTabs(true)
    session.setTabSize(2)
    if (this.mode) {
      session.setMode('ace/mode/' + this.mode)
    }
    editor.setSession(session)

    editor.on('change', async (evt) => {
      // console.log('change', evt)
      if (this.lock) {
        this.lock = false
        return
      }
      // console.log('ace change')
      // console.log(e)
      const editorValue = editor.getValue()

      this.context.output.value = editorValue
      this.$emit('change', editorValue)
    })

    this.editor = editor
  }
}
</script>
