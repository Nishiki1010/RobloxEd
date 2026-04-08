# Lesson 6: スコアを数えよう

**Phase 2 — コイン集めアドベンチャー 第3回 | コード: あり | 80分**

---

## 今日のゴール

> コインを取ったらスコアが増える仕組みを作ろう！
> 「何個取ったか」がわかるようにするよ。

**完成イメージ**: コインを取るたびに Output に「Score: 1」「Score: 2」...と表示。全部取ったら「CLEAR!」

---

## 前回の復習（3分）

「前回はコインを触ったら消えるようにしたよね。でも今のままだと、何個取ったかわからない。今日はそれを解決するよ！」

---

## チャレンジ

### チャレンジ 1: 問題を考えよう（5分）

**ミッション**: 「今のゲーム、何が足りないと思う？」

**講師の問いかけ**:
- 「コイン取ったけど、何個取ったかわかる？」→「わからない」
- 「じゃあ"数える"仕組みが必要だよね」
- 「Lesson 3 で"変数"ってやったの覚えてる？あれを使うよ」

---

### チャレンジ 2: スコア変数を作ろう（10分）

**ミッション**: 「コインの数を数えるための"箱"を用意しよう」

**導入**: 「変数は"名前のついた箱"だったよね。今回は `score` っていう箱を作って、最初は 0 にするよ」

**今のコイン1個のコードを修正**:

```lua
local coin = script.Parent
local score = 0

coin.Touched:Connect(function(hit)
    local humanoid = hit.Parent:FindFirstChild("Humanoid")
    if humanoid then
        score = score + 1
        print("Score: " .. score)
        coin:Destroy()
    end
end)
```

**新しい部分を説明**:

| コード | 意味 |
|--------|------|
| `local score = 0` | スコアの箱を作って、最初は 0 |
| `score = score + 1` | スコアに 1 を足す |
| `print("Score: " .. score)` | 「Score: 〇」と表示 |

**たとえ話**: 「`score = score + 1` って変な式に見えるよね。これは"今のスコアに1を足して、また同じ箱に入れる"って意味。数学の = とは違うんだ」

**想定つまづき（講師用）**:
- `score = score + 1` が理解できない → 「箱の中身を出す → 1を足す → また箱に戻す」と説明
- 1個目は動くが2個目以降スコアが増えない → 各コインに別々の score 変数がある問題。これは想定通り。チャレンジ 3 へ

---

### チャレンジ 3: 問題発見 — スコアが共有されない！（10分）

**ミッション**: 「Play して複数のコインを取ってみよう。スコアはちゃんと増える？」

**起きること**: 各コインのスクリプトに別々の `score` 変数があるため、どのコインを取っても「Score: 1」としか表示されない。

**講師の声かけ**:
- 「あれ？2個目を取っても Score: 1 のまま？なんでだろう？」
- 「各コインが自分の score を持ってるからだよ。みんなバラバラに数えてる」
- 「全コインが同じスコアを見るようにしたいよね」

---

### チャレンジ 4: IntValue でスコアを共有しよう（20分）

**ミッション**: 「全部のコインが同じスコアを見られる仕組みを作ろう」

**IntValue とは**: 「数字を保存できる特別な入れ物。Workspace に置いておけば、全部のコインから見える」

**手順**:
1. Workspace の中に IntValue を追加（＋ → IntValue）
2. 名前を `Score` に変更
3. Value を 0 に設定

**コインのコードを修正**:

```lua
local coin = script.Parent
local scoreValue = workspace:FindFirstChild("Score")

coin.Touched:Connect(function(hit)
    local humanoid = hit.Parent:FindFirstChild("Humanoid")
    if humanoid then
        scoreValue.Value = scoreValue.Value + 1
        print("Score: " .. scoreValue.Value)
        coin:Destroy()
    end
end)
```

**新しい部分を説明**:

| コード | 意味 |
|--------|------|
| `workspace:FindFirstChild("Score")` | Workspace の中から "Score" を探す |
| `scoreValue.Value` | IntValue の中の数字 |
| `scoreValue.Value = scoreValue.Value + 1` | 共有のスコアに 1 を足す |

**テスト**: Play → コインを複数取る → Score: 1, 2, 3... と増える！

**成功の瞬間**: 「やった！ちゃんと数えてる！全部のコインが同じスコアを見てるんだ」

---

### チャレンジ 5: クリア条件を作ろう（15分）

**ミッション**: 「全部のコインを取ったら "CLEAR!" って表示させよう」

**考えさせる**: 「全部で何個コイン置いた？じゃあスコアがその数になったらクリアだよね」

```lua
local coin = script.Parent
local scoreValue = workspace:FindFirstChild("Score")
local totalCoins = 5  -- コインの総数に合わせて変える

coin.Touched:Connect(function(hit)
    local humanoid = hit.Parent:FindFirstChild("Humanoid")
    if humanoid then
        scoreValue.Value = scoreValue.Value + 1
        print("Score: " .. scoreValue.Value)
        coin:Destroy()

        if scoreValue.Value >= totalCoins then
            print("🎉 CLEAR! おめでとう！ 🎉")
        end
    end
end)
```

**新しい部分**:

| コード | 意味 |
|--------|------|
| `if scoreValue.Value >= totalCoins then` | 「もしスコアがコインの総数以上なら」 |
| `>=` | 「以上」（大きいか同じ） |

**テスト**: 全コインを集める → 「CLEAR!」が表示される！

---

## 振り返り（最後の5分）

1. **「変数って何だったっけ？自分の言葉で説明して」**
2. **「スコアが共有されない問題、どうやって解決した？」**
3. **「Output じゃなくて、ゲーム画面にスコアを表示したくない？次回やるよ！」**

→ 回答を **授業ログDB** に記録する

---

## 講師メモ（授業前3分で読む）

### この回の核心
**「問題に気づく → 原因を考える → 解決する」** のサイクルを意図的に体験させる。チャレンジ 2 であえて「うまくいかない方法」を先にやらせ、チャレンジ 3 で問題を発見させ、チャレンジ 4 で解決する流れ。

### 超重要: あえて失敗させる
チャレンジ 2 の時点で「これだと各コインが別々に数えるから共有されないよ」と先に言わない。**生徒自身が Play して「あれ？」と気づく体験**が最も重要。

### IntValue の説明
「教室の壁に貼ってあるスコアボードみたいなもの。みんなが見える場所にあるから、誰がコインを取ってもスコアボードの数字が増える」

### NG対応
- ❌ 最初から IntValue を使った完成版を見せる
- ❌ 「各コインに別々の変数があるから共有されない」と先に説明する

### OK対応
- ✅ 「Play して確かめてみて。ちゃんと増える？」
- ✅ 「なんで Score: 1 のままなんだろう？考えてみて」
- ✅ 「全員が見えるスコアボードを作ろう！」

---

## 補助資料
- 公式: [IntValue](https://create.roblox.com/docs/reference/engine/classes/IntValue)
- 公式: [Variables](https://create.roblox.com/docs/scripting/luau/variables)
