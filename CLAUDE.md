# stemplay

輕量級 web 分軌播放器。

## 目標

讓 demucs 拆好的分軌可以直接拖曳到網頁上同步播放，不用每次都開 Logic。

## 技術方向

- 純前端，單一 HTML
- Web Audio API 同步多軌播放
- 拖曳載入本地檔案（不上傳、不需後端）
- 每軌獨立音量 / mute / solo
- 時間軸 seek

## 功能

- [ ] 拖曳多個音檔載入
- [ ] 同步播放 / 暫停 / seek
- [ ] 每軌音量 / mute / solo
- [ ] 時間顯示 / 波形（之後再說）

## 已知問題 / TODO

### AirPods 播放控制（暫緩）

目標：戴 AirPods 時可以用耳機按鈕暫停與播放。

**現狀**：尚未實作，已回退相關嘗試。

**試過的方向**：
1. 靜音 `<audio>` anchor（0-byte WAV data URL）＋ Media Session API：AirPods 可暫停，但**按播放無反應**。推測是 0-byte WAV 不被瀏覽器視為有效 media。
2. 改用 runtime 生成的 1 秒有效靜音 WAV：AirPods 按播放時會把指令送到**其他有音訊的分頁**（例如 Brave 另一個 tab 的 YouTube），因為靜音 tab 在 media key 路由優先度上輸給有聲音的 tab。
3. 用 `MediaStreamDestination` 把整個 mix 路由到 anchor（讓 anchor 變成有真實音訊的 media element）：在 Brave 上完全沒聲音，音訊輸出鏈似乎斷掉。

**可能的下一步**：
- 在 Chrome（非 Brave）測試 MediaStream 方案是否可行
- 或改成雙路輸出：`masterGain` 同時接 `ctx.destination`（聽得到）＋ `MediaStreamDestination`（餵給 anchor）
- 或接受限制，只做網頁內部的快捷鍵控制
