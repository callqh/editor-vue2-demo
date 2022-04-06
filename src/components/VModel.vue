<template>
  <div>
    <textarea v-model="valueHtml" style="width: 100%; height: 100px"></textarea>
    <div style="border: 1px solid #ccc; margin-top: 10px">
      <Toolbar
        style="border-bottom: 1px solid #ccc"
        :editor="editor"
        :defaultConfig="toolbarConfig"
      />
      <Editor
        style="height: 500px"
        :defaultConfig="editorConfig"
        v-model="valueHtml"
        @onCreated="onCreated"
      />
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import "@wangeditor/editor/dist/css/style.css";
import { Editor, Toolbar } from "@wangeditor/editor-for-vue";

export default Vue.extend({
  components: { Editor, Toolbar },
  data() {
    return {
      editor: null,
      toolbarConfig: {
        // 工具栏配置
      },
      editorConfig: {
        placeholder: "请输入内容...",
      },
      valueHtml: "<p>hello</p>",
    };
  },
  mounted() {
    setTimeout(() => {
      this.valueHtml = "<p>hello world</p>";
    }, 1500);
  },
  methods: {
    onCreated(editor) {
      this.editor = editor;
    },
  },

  // 及时销毁 editor
  beforeDestroy() {
    const editor = this.editor;
    if (editor == null) return;
    w;
    // 销毁，并移除 editor
    editor.destroy();
  },
});
</script>
