# Lesson 7: 画面にスコアを表示しよう

**Phase 2 — コイン集めアドベンチャー 第4回 | コード: あり | 80分**

---

## 今日のゴール

> Output じゃなくて、ゲーム画面にスコアを表示しよう！
> 本物のゲームみたいに、画面の上にスコアが出るようにするよ。

**完成イメージ**: 画面上部に「コイン: 3 / 5」のようなスコア表示。コインを取るとリアルタイムで更新される。

---

## 前回の復習（3分）

「前回は IntValue でスコアを共有して、Output に表示したよね。でも本物のゲームって Output じゃなくて画面にスコアが出るよね？今日はそれを作るよ！」

---

## チャレンジ

### チャレンジ 1: GUI って何？（5分）

**ミッション**: 「ゲームの画面に文字を出す仕組みを見つけよう」

**講師の説明**:
「ゲームをやっていると、HPバーとかスコアとかが画面に出てるよね。あれを GUI（グイ）って言うよ。Roblox でも作れるんだ」

**GUI の構造（たとえ話）**:
- ScreenGui = 画面に貼る「透明なシート」
- TextLabel = シートの上に置く「テキストのシール」

---

### チャレンジ 2: ScreenGui と TextLabel を作ろう（15分）

**ミッション**: 「画面に文字を表示してみよう！」

**手順**:
1. Explorer → StarterGui を見つける
2. StarterGui の ＋ → ScreenGui を追加
3. ScreenGui の ＋ → TextLabel を追加
4. TextLabel の Properties を設定:
   - Text: `コイン: 0`
   - TextSize: `24`
   - TextColor3: 白（1, 1, 1）
   - BackgroundTransparency: `1`（背景を透明に）
   - Position: 画面上部（UDim2 で調整）
   - Font: `GothamBold`

**ヒント**:
- Lv.1: 「StarterGui っていうフォルダが Explorer にあるよ。ここに画面の部品を入れるんだ」
- Lv.2: 「ScreenGui → TextLabel の順で追加してね」
- Lv.3: 一緒に Properties を設定する

**テスト**: Play → 画面に「コイン: 0」が表示される

---

### チャレンジ 3: スコアとGUIを連動させよう（25分）

**ミッション**: 「コインを取ったら、画面の数字が増えるようにしよう！」

**考えさせる**: 「IntValue の値が変わったら、TextLabel の文字を更新したい。どうすればいい？」

**方法**: StarterGui の中に LocalScript を作る

**手順**:
1. ScreenGui の中に LocalScript を追加
2. 以下のコードを書く:

```lua
local scoreValue = workspace:WaitForChild("Score")
local textLabel = script.Parent:WaitForChild("TextLabel")
local totalCoins = 5  -- コインの総数

-- 表示を更新する関数
local function updateDisplay()
    textLabel.Text = "コイン: " .. scoreValue.Value .. " / " .. totalCoins
end

-- 最初の表示
updateDisplay()

-- スコアが変わるたびに更新
scoreValue.Changed:Connect(updateDisplay)
```

**新しい概念を説明**:

| コード | 意味 |
|--------|------|
| `WaitForChild("Score")` | 「Score が出てくるまで待つ」（安全な探し方） |
| `local function updateDisplay()` | 「表示を更新する」という名前の命令セットを作る |
| `scoreValue.Changed:Connect(...)` | 「スコアが変わったら updateDisplay を実行」 |

**たとえ話**:
- `function` = 「レシピ」。一度書いておけば何回でも使える
- `Changed` = 「値が変わったよ！」というお知らせ。Touched と似てるね

**想定つまづき（講師用）**:
- LocalScript vs Script → 「LocalScript は画面に関することを担当。Script はゲーム世界のことを担当。今はそれだけ覚えればOK」
- WaitForChild → 「FindFirstChild の親戚。"見つかるまで待つ"バージョン」

---

### チャレンジ 4: テストプレイで確認しよう（10分）

**ミッション**: 「Play してコインを集めてみよう。画面の数字がちゃんと増える？」

**チェックリスト**:
- [ ] 最初に「コイン: 0 / 5」と表示される
- [ ] コインを取るたびに数字が増える
- [ ] 全部取ったら「コイン: 5 / 5」になる

**問題があったら**:
- 「表示が変わらない？→ LocalScript が ScreenGui の中にあるか確認」
- 「数字がおかしい？→ totalCoins の数がコインの実数と合ってるか確認」

---

### チャレンジ 5: クリア演出を追加しよう（15分）

**ミッション**: 「全コイン取得したら、画面にクリアメッセージを出そう！」

**LocalScript に追加**:

```lua
scoreValue.Changed:Connect(function()
    updateDisplay()
    if scoreValue.Value >= totalCoins then
        textLabel.Text = "🎉 CLEAR! 🎉"
        textLabel.TextSize = 48
        textLabel.TextColor3 = Color3.fromRGB(255, 215, 0)  -- 金色
    end
end)
```

> ※ Changed の接続を上記に差し替える

**テスト**: 全コイン収集 → 画面に大きく「CLEAR!」が金色で表示される！

---

## 振り返り（最後の5分）

1. **「GUI って何だったっけ？」**
2. **「function って何のために使う？」**
3. **「次回は罠を追加するよ。どんな罠がいい？」**

→ 回答を **授業ログDB** に記録する

---

## 講師メモ（授業前3分で読む）

### この回の核心
**「ゲームっぽさ」が一気に増す回**。Output から画面上のGUIに変わることで、「本物のゲームっぽい！」という感動がある。この体験がモチベーションを大きく上げる。

### function の導入
初めての function だが、深く説明しすぎない。「何回も使う処理に名前をつけておくと便利」くらいでOK。

### LocalScript について
Server/Client の違いを厳密に教えるのは Phase 3 以降。今は「画面のことは LocalScript」「ゲーム世界のことは Script」でOK。

### NG対応
- ❌ Server/Client アーキテクチャの詳細を説明する
- ❌ 「function はこういう構文で...」と文法の説明から入る

### OK対応
- ✅ 「Output から画面に変わっただけで、ゲームっぽくなったでしょ！」
- ✅ 「function = レシピ。名前つけて何回でも使える」
- ✅ 「Changed = "変わったよ！"のお知らせ」
