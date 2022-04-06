<template>
  <div v-if="flag" style="border: 1px solid #ccc">
    <Toolbar
      :editorId="editorId"
      :defaultConfig="toolbarConfig"
      :mode="mode"
      style="border-bottom: 1px solid #ccc"
    />
    <Editor
      :editorId="editorId"
      :defaultConfig="editorConfig"
      :defaultContent="defaultContent"
      :defaultHtml="defaultHtml"
      :mode="mode"
      style="height: 500px; overflow-y: hidden"
    />
    <!-- 注意: defaultContent (JSON 格式) 和 defaultHtml (HTML 格式) ，二选一 -->
  </div>
</template>

<script lang="ts">
import { onBeforeUnmount, ref, defineComponent } from 'vue';
import { Editor, Toolbar, getEditor, removeEditor } from '@wangeditor/editor-for-vue';
import '@wangeditor/editor/dist/css/style.css';

export default defineComponent({
  components: { Editor, Toolbar },
  setup() {
    const editorId = `w-e-${Math.random().toString().slice(-5)}`; //【注意】编辑器 id ，要全局唯一
    const flag = ref(false);
    // defaultContent (JSON 格式) 和 defaultHtml (HTML 格式) ，二选一
    const defaultHtml = ref('<p>一行文字</p>');
    const defaultContent = ref([{ type: 'paragraph', children: [{ text: '一行文字content' }] }]);
    // const getDefaultContent = computed(() => cloneDeep(defaultContent)); // 注意，要深拷贝 defaultContent ，否则报错

    const toolbarConfig = {};
    const editorConfig = { placeholder: '请输入内容...' };
    setTimeout(() => {
      // defaultContent (JSON 格式) 和 defaultHtml (HTML 格式) ，二选一
      defaultHtml.value = '<p>hello&nbsp;<strong>world</strong></p>\n<p><br></p>';
      // defaultContent.value = [{ type: 'paragraph', children: [{ text: 'ajax 异步获取的内容' }] }];
      flag.value = true;
    }, 5000);

    // 组件销毁时，也及时销毁编辑器
    onBeforeUnmount(() => {
      const editor = getEditor(editorId);
      if (editor == null) return;

      editor.destroy();
      removeEditor(editorId);
    });

    return {
      editorId,
      mode: 'default',
      defaultHtml,
      defaultContent,
      toolbarConfig,
      editorConfig,
      flag,
    };
  },
});
</script>
