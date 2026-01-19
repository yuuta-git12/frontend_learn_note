## ① 初回レンダリング時の流れ

```js
function Counter() {   
	const [count, setCount] = useState(0);
	return <button>{count}</button>; 
}
```


### 処理の流れ（初回）
```
[React]
  ↓ Counterを呼ぶ
[Counter関数 実行]
  ↓ useState(0)
[React内部]
  └ state = 0 を記憶
  ↓
[count = 0] が返る
  ↓
return <button>0</button>
  ↓
[画面に描画]

```

### イメージ図
```
┌──────────────┐
│ Counter関数  │
│ count = 0    │
└─────┬────────┘
      ↓
   <button>0</button>
      ↓
   画面に表示

```

---

## ② ボタンを押したとき（setStateが呼ばれる）

`<button onClick={() => setCount(count + 1)}>`

### 重要ポイント

❌ setCount は **即座に count を書き換えない**  
⭕ Reactに「次はこの値で再レンダリングして」と **予約** する

---

## ③ setState → 再レンダリングまでの流れ

### 処理の流れ（ここが核心）

```
[ユーザー操作]  
↓ 
setCount(1)    
↓ 
[React]   
    └ state を 0 → 1 に更新    
↓ 
「このコンポーネントを再レンダリングしよう」
↓ 
Counter関数をもう一度 実行`

```
### 図で見ると
```
setCount(1)
   ↓
┌─────────────────┐
│ React内部 state │
│ count: 1        │
└──────┬──────────┘
       ↓
  Counter関数 再実行

```


---

## ④ 再レンダリング時に何が起きるか

### 再び Counter が実行される
```
[Counter関数 実行（2回目）]
  ↓
useState(0) ← 初期値は無視！
  ↓
[count = 1] が返る（Reactが覚えている）
  ↓
return <button>1</button>

```

### 図で整理
```
┌──────────────┐
│ Counter関数  │
│ count = 1    │
└─────┬────────┘
      ↓
   <button>1</button>

```

---

## ⑤ 画面はどう更新される？

Reactは **Virtual DOM** を使います。
```
前回の描画: <button>0</button>
今回の描画: <button>1</button>
```


👇
```
差分だけ発見
  ↓
DOMの「0 → 1」だけ更新
```
❗ DOMを全部作り直しているわけではありません

---

## ⑥ 全体の流れを1枚でまとめると
```
① 初回表示
React → Counter実行 → JSX → 画面表示

② ユーザー操作
クリック
  ↓
setCount
  ↓
Reactがstate更新
  ↓
Counter再実行
  ↓
新しいJSX生成
  ↓
差分だけDOM更新
```
---

## ⑦ よく混乱するポイント（超重要）

### ❌ 勘違いしやすい考え

`setCount → countがその場で変わる`

### ⭕ 正しい考え方

`setCount   ↓ 「次のレンダリングで使う値」をReactに渡す`

だからこうなる👇

`setCount(count + 1); console.log(count); // まだ古い値`

---

## ⑧ たとえ話（かなり重要）

Reactコンポーネントは **関数 = 設計図**

`state = 材料 関数 = レシピ render = 料理`

材料（state）が変わったら  
👉 **レシピを最初から作り直す**  
👉 でも **変わった部分だけ盛り付け直す**