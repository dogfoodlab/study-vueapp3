<template>
  <div>

    <Editor
      width="100%"
      height="300px"
      theme="solarized_light"
      mode="json"
      :context="editorContext"
      @change="onChangeEditor"
    />

    <v-row>
      <v-col cols="3">
        <v-text-field v-model="inputText" />
      </v-col>
      <v-col>
        <v-btn @click="setEditorText">Set</v-btn>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="3">
        <v-text-field v-model="editorContext.output.value" />
      </v-col>
    </v-row>

  </div>
</template>

<script>
import Editor from '@/components/Editor'

export default {
  components: {
    Editor
  },
  data () {
    return {
      inputText: '',
      editorContext: { input: { value: '' }, output: { value: '' } }
    }
  },
  methods: {
    async setEditorText () {
      // console.log('setEditorText', this.inputText)
      this.editorContext.input = { value: this.inputText }
    },
    async onChangeEditor (text) {
      // this.editorContext.output.value = text
      console.log('onChangeEditor', text)
    }
  }
}
</script>
