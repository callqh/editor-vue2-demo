<template>
  <div style="border: 1px solid #ccc">
    <!-- 工具栏 -->
    <Toolbar
      style="border-bottom: 1px solid #ccc"
      :editorId="editorId"
      :defaultConfig="toolbarConfig"
      :mode="mode"
    />
    <!-- 编辑器 -->
    <Editor
      style="height: 300px"
      :editorId="editorId"
      :defaultConfig="editorConfig"
      :defaultContent="getDefaultContent"
      @onChange="onChange"
      @onCreated="onCreated"
    ></Editor>
  </div>
</template>

<script>
import '@wangeditor/editor/dist/css/style.css';
import { Editor, Toolbar, getEditor, removeEditor } from '@wangeditor/editor-for-vue';
import cloneDeep from 'lodash.clonedeep';

export default {
  name: 'WangEditor',
  components: { Editor, Toolbar },
  props: {
    defaultContent: {
      type: Array,
      default: () => [],
    },
  },
  mounted() {},
  data() {
    return {
      mode: 'default',
      editorId: 'wangEditor-1', // 定义一个编辑器 id ，要求：全局唯一且不变。重要！！！
      toolbarConfig: {
        excludeKeys: ['fullScreen'],
      },
      editorConfig: {
        placeholder: '',
        MENU_CONF: {
          uploadImage: {
            server: 'api/upload',
            fieldName: 'file',
            meta: {
              type: 'image',
            },
            // 选择文件时的类型限制，默认为 ['image/*'] 。如不想限制，则设置为 []
            allowedFileTypes: ['image/*'],
            headers: {
              Authorization: 'a',
            },
            maxFileSize: 10 * 1024 * 1024, // 10M
            // 自定义插入图片
            customInsert(res, insertFn) {
              // res 即服务端的返回结果
              const url = 'http://101.35.5.235:81' + res.data;
              console.log(url, '===');
              // 从 res 中找到 url alt href ，然后插图图片
              insertFn(url);
            },
            // 上传之前触发
            onBeforeUpload(files) {
              // files 即选中的文件列表

              return files;

              // 返回值可选择：
              // 1. 返回一个数组（files 或者 files 的一部分），则将上传返回结果中的文件
              // 2. 返回 false ，则终止上传
            },
            // 上传进度的回调函数
            onProgress(progress) {
              // progress 是 0-100 的数字
              console.log('progress', progress);
            },
            // 单个文件上传成功之后
            onSuccess(file, res) {
              console.log(`${file.name} 上传成功`, res);
            },
            // 单个文件上传失败
            onFailed(file, res) {
              console.log(`${file.name} 上传失败`, res);
            },
            // 上传错误，或者触发 timeout 超时
            onError(file, err, res) {
              console.log(`${file.name} 上传出错`, err, res);
            },
          },
        },
      },
    };
  },
  computed: {
    getDefaultContent() {
      return cloneDeep(this.defaultContent); // 深拷贝，重要！！！
    },
  },
  methods: {
    onCreated(editor) {
      const a = editor.getMenuConfig('fontSize');
      const b = editor.getMenuConfig('uploadImage');
      console.log(a);
      console.log(b);
    },
    onChange(editor) {
      this.$emit('change', JSON.stringify(editor.children), editor.getHtml());
    },
  },
  beforeDestroy() {
    const editor = getEditor(this.editorId);
    if (editor == null) return;
    editor.destroy(); // 组件销毁时，及时销毁编辑器 ，重要！！！
    removeEditor(this.editorId);
  },
};
</script>
