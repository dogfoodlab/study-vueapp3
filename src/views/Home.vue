<template>
  <div>

    <Editor
      width="100%"
      height="300px"
      :context="editorContext"
      @change="onChangeEditor"
    />

    <div>
      <v-text-field v-model="inputText" />
      <v-btn @click="setEditorText">test</v-btn>
    </div>

    <div>
      <v-text-field v-model="editorContext.value" />
    </div>

  </div>
</template>

<script>
import Editor from '@/components/Editor'

export default {
  components: {
    Editor
  },
  data: () => ({
    inputText: '',
    editorContext: { value: undefined }
  }),
  methods: {
    async setEditorText () {
      // console.log('setEditorText', this.inputText)
      this.editorContext = { value: this.inputText }
    },
    async onChangeEditor (text) {
      console.log('onChangeEditor', text)
    }
  }
}
</script>
