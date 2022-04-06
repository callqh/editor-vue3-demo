<template>
  <div>
    <textarea v-model="valueHtml" style="width: 100%; height: 150px"></textarea>
  </div>
  <div style="border: 1px solid #ccc; margin-top: 20px">
    <Toolbar :editorId="editorId" style="border-bottom: 1px solid #ccc" />
    <Editor
      :editorId="editorId"
      :defaultConfig="editorConfig"
      v-model="valueHtml"
      style="height: 300px"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onBeforeUnmount, onMounted } from 'vue';
import { Editor, Toolbar, getEditor, removeEditor } from '@wangeditor/editor-for-vue';
import '@wangeditor/editor/dist/css/style.css';

export default defineComponent({
  components: {
    Editor,
    Toolbar,
  },
  setup() {
    // 编辑器唯一id值
    const editorId = 'we-1002';

    const valueHtml = ref('<p>hello</p>');

    // 编辑器相关配置
    const editorConfig = {
      placeholder: '请输入内容...',
    };

    onMounted(() => {
      // 模拟 ajax 异步设置 value
      setTimeout(() => {
        valueHtml.value = '<p>hello world</p>'; // 测试 v-model
      }, 2000);
    });

    // 组件销毁时，也及时销毁编辑器
    onBeforeUnmount(() => {
      const editor = getEditor(editorId);
      console.log(
        '%c 🍬 editor:',
        'font-size:22px;background-color:rgb(138, 152, 142);color:#fff;',
        editor
      );
      if (editor == null) return;

      editor.destroy();
      removeEditor(editorId);
    });

    return {
      editorId,
      editorConfig,
      valueHtml,
    };
  },
});
</script>
