<template>
  <main class="container">
    <h1>📝 工作日志记录</h1>

    <form @submit.prevent="submitLog">
      <input v-model="logContent" placeholder="请输入今日工作内容..." />
      <button type="submit">记录</button>
    </form>

    <p v-if="message">{{ message }}</p>
  </main>
</template>

<script setup lang="ts">
import { invoke } from '@tauri-apps/api/tauri'
import { ref } from 'vue'

const logContent = ref("")
const message = ref("")

async function submitLog() {
  if (!logContent.value.trim()) {
    message.value = "请输入内容再提交。"
    return
  }

  await invoke("write_log", { content: logContent.value })
  message.value = "✅ 已记录：" + logContent.value
  logContent.value = ""
}
</script>
