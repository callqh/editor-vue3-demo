<script setup lang="ts">
import { ref } from "vue";
import Editor from "./components/Editor.vue";
import VModel from "./components/VModel.vue";

const editorType = ref<"base" | "vmodel">("base");
/**
 * 切换编辑器类型
 * @param type base | vmodel
 */
const changeType = (type: "base" | "vmodel") => {
  editorType.value = type;
};
</script>

<template>
  <div>
    <div>
      <button @click="changeType('base')">Base Editor</button>
      <button @click="changeType('vmodel')">V-Model Editor</button>
    </div>
    <div>
      <Editor v-if="editorType === 'base'" />
      <VModel v-if="editorType === 'vmodel'" />
    </div>
  </div>
</template>
