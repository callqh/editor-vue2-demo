<template>
  <div>
    <div>
      <button @click="insertText">insert text</button>
    </div>
    <div style="border: 1px solid #ccc">
      <!-- 工具栏 -->
      <!-- <Toolbar
        :editor="editor"
        style="border-bottom: 1px solid #ccc"
        v-bind:editorId="editorId"
        :defaultConfig="toolbarConfig"
        :mode="mode"
      /> -->

      <!-- 编辑器 -->
      <Editor
        style="height: 500px"
        :defaultHtml="defaultHtml"
        :defaultConfig="editorConfig"
        :defaultContent="defaultContent"
        :mode="mode"
        @onCreated="onCreated"
        @onChange="onChange"
      />
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import "@wangeditor/editor/dist/css/style.css";
import { Editor, Toolbar } from "@wangeditor/editor-for-vue";

export default Vue.extend({
  components: { 
    Editor,
  //  Toolbar 
   },
  data() {
    return {
      //【特别注意】
      // 1. editorId Toolbar 和 Editor 的关联，要保持一致
      // 2. 多个编辑器时，每个的 editorId 要唯一
      editorId: "w-e-1",
      editor: null,
      toolbarConfig: {
        /* 工具栏配置 */
      },
      defaultHtml: "<h1>这是一个默认的内容</h1>",
      defaultContent: [
        {
          type: "paragraph",
          children: [
            {
              type: "image",
              src: "https://www.keaidian.com/uploads/allimg/190424/24110307_7.jpg",
              href: "",
              alt: "",
              style: { width:'100px'},
              children: [
                {
                  text: "",
                },
              ],
            },
          ],
        },
        {
          type: "paragraph",
          children: [
            {
              text: "简化 toolbar 和 hoverbar",
            },
          ],
        },
        {
          type: "pre",
          children: [
            {
              type: "code",
              language: "java",
              children: [
                {
                  text: "public static void main(String args[]){String name = ''; System.out.println(name);}",
                },
              ],
            },
          ],
        },
      ],
      editorConfig: {
        readOnly: true,
        placeholder: "请输入内容...",
        // 其他编辑器配置
        // 菜单配置
        // MENU_CONF: {
        //   uploadImage: {
        //     server: "http://106.12.198.214:3000/api/upload-img",
        //     // form-data fieldName ，默认值 'wangeditor-uploaded-file'
        //     fieldName: "your-custom-name",

        //     // 单个文件的最大体积限制，默认为 2M
        //     maxFileSize: 10 * 1024 * 1024, // 1M

        //     // 最多可上传几个文件，默认为 100
        //     maxNumberOfFiles: 10,

        //     // 选择文件时的类型限制，默认为 ['image/*'] 。如不想限制，则设置为 []
        //     allowedFileTypes: ["image/*"],

        //     // 将 meta 拼接到 url 参数中，默认 false
        //     metaWithUrl: false,

        //     // 自定义增加 http  header
        //     headers: {
        //       Accept: "text/x-json",
        //       otherKey: "xxx",
        //     },

        //     // 跨域是否传递 cookie ，默认为 false
        //     withCredentials: false,

        //     // 超时时间，默认为 10 秒
        //     timeout: 5 * 1000, // 5 秒

        //     // 小于该值就插入 base64 格式（而不上传），默认为 0
        //     base64LimitSize: 5 * 1024, // 5kb
        //     meta: {
        //       token: "xxx",
        //       a: 100,
        //     },
        //     // 自定义插入图片
        //     customInsert(res, insertFn) {
        //       console.log(res, "===");
        //       // res 即服务端的返回结果
        //       res = res.data;
        //       // 从 res 中找到 url alt href ，然后插图图片
        //       insertFn(res.url, res.alt, res.href);
        //     },
        //     // 上传之前触发
        //     onBeforeUpload(files) {
        //       // files 即选中的文件列表

        //       return files;

        //       // 返回值可选择：
        //       // 1. 返回一个数组（files 或者 files 的一部分），则将上传返回结果中的文件
        //       // 2. 返回 false ，则终止上传
        //     },
        //     // 上传进度的回调函数
        //     onProgress(progress) {
        //       // progress 是 0-100 的数字
        //       console.log("progress", progress);
        //     },
        //     // 单个文件上传成功之后
        //     onSuccess(file, res) {
        //       console.log(`${file.name} 上传成功`, res);
        //     },
        //     // 单个文件上传失败
        //     onFailed(file, res) {
        //       console.log(`${file.name} 上传失败`, res);
        //     },
        //     // 上传错误，或者触发 timeout 超时
        //     onError(file, err, res) {
        //       console.log(`${file.name} 上传出错`, err, res);
        //     },
        //   },
        // },
      },
      mode: "default", // or 'simple'
      curContent: [],
    };
  },

  methods: {
    onCreated(editor) {
      this.editor = Object.seal(editor);
      console.log("disable ?", editor.isDisabled());
      console.log("config", editor.getConfig());
      console.log("onCreated ", editor);
    },
    onChange(editor) {
      console.log("onChange", editor.children);
      this.curContent = editor.children;
    },
    onDestroyed(editor) {
      console.log("onDestroyed", editor);
    },
    onMaxLength(editor) {
      console.log("onMaxLength", editor);
    },
    onFocus(editor) {
      console.log("onFocus", editor);
    },
    onBlur(editor) {
      console.log("onBlur", editor);
    },
    customAlert(info, type) {
      window.alert(`customAlert in Vue demo\n${type}:\n${info}`);
    },
    customPaste(editor, event, callback) {
      console.log("ClipboardEvent 粘贴事件对象", event);

      // 自定义插入内容
      editor.insertText("xxx");

      // 返回值（注意，vue 事件的返回值，不能用 return）
      // callback(false); // 返回 false ，阻止默认粘贴行为
      callback(true); // 返回 true ，继续默认的粘贴行为
    },

    insertText() {
      // 获取 editor 实例，即可执行 editor API
      const editor = this.editor;
      if (editor == null) return;
      if (editor.selection == null) return;

      // 在选区插入一段文字
      editor.insertText("一段文字");
    },
  },

  // 及时销毁 editor
  beforeDestroy() {
    const editor = this.editor;
    if (editor == null) return;

    // 销毁，并移除 editor
    editor.destroy();
  },
});
</script>
