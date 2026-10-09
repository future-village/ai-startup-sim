# 給 AI 代理：怎麼玩《AI 新創養成》

這個遊戲是一個靜態網頁，規則全部是種子化的純函式，AI 可以用兩種方式玩。

## 方式一：直接呼叫遊戲邏輯（最穩）
頁面載入後，`window.AgentTycoonLogic` 提供整套規則：

```js
const L = window.AgentTycoonLogic;
let s = L.defaultState("AB12CD");           // 開新局（6 碼種子）
s = L.chooseDecision(s, "fundraise");        // 季決策：hire / consult / fundraise / compute / polish / hype
while (s.phase === "event") {                // 處理本季事件
  const ev = L.getCurrentEvent(s);
  const opts = L.eventOptions(ev);           // 每個選項有 label 與 effects
  s = L.resolveEvent(s, 0);                  // 選第 0 個選項
}
console.log(s.quarter, s.stats, s.ending);   // ending：ipo / acquired / quiet_exit / retired / burnout / bankrupt
```

- `L.previewDecision(s, id).decision.effects`：選之前先看這張卡實際會改哪些數字（含募資稀釋）。
- `L.quarterEconomy(s)`：本季預估收入、燒錢、維護成本。
- `L.simulate(seed)`：內建自動玩家跑完一整局，可當基準線比較你的策略。
- 同一個種子、同一串選擇，結果完全一樣，可以重播與比較。

## 方式二：點畫面
- 季決策按鈕：`[data-decision="hire"]` 等六個。
- 事件選項：`[data-event-option="0"]`、`[data-event-option="1"]`。
- 結果頁的繼續按鈕：`#continueResult`。
- 會直接結束這局的選項要按兩次（第一次只會亮紅框）。

## 請遵守
- 這是給人玩的遊戲，請不要對網站大量重複請求；邏輯在本機跑，不需要連線。
- 進度只存在瀏覽器 localStorage，不會上傳任何資料。
