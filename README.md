# walkme-calculator-unofficial
WalkMeのSmart Walk-Thrusで画面遷移し、UIもしくはURLパラメータ上を使って各種演算ができるWebアプリケーション

# WalkMe Calculator - URLパラメータ一覧

## 📋 基本パラメータ

### 数値計算用パラメータ
| パラメータ名 | 説明 | 例 | 必須 |
|-------------|------|----|----|
| `numA` | 計算対象の数値A | `10`, `10.5`, `-5` | ○ |
| `numB` | 計算対象の数値B | `5`, `2.3`, `-10` | ○ |
| `operation` | 演算種類 | `add`, `subtract`, `multiply` | ○ |

### テキスト処理用パラメータ
| パラメータ名 | 説明 | 例 | 必須 |
|-------------|------|----|----|
| `text1` | テキスト1 | `Hello`, `WalkMe` | - |
| `text2` | テキスト2 | `World`, `Calculator` | - |
| `separator` | 区切り文字 | ` ` (スペース), `,`, `_` | - |

## 🔧 operationパラメータのオプション値

| 値 | 演算 | 説明 | 例 |
|---|-----|------|-----|
| `add` | 加算 (A + B) | 数値Aと数値Bを足し算 | `numA=10&numB=5` → `15` |
| `subtract` | 減算 (A - B) | 数値Aから数値Bを引き算 | `numA=10&numB=3` → `7` |
| `multiply` | 乗算 (A × B) | 数値Aと数値Bを掛け算 | `numA=4&numB=5` → `20` |
| `divide` | 除算 (A ÷ B) | 数値Aを数値Bで割り算 | `numA=20&numB=4` → `5` |
| `power` | べき乗 (A ^ B) | 数値AのB乗 | `numA=2&numB=3` → `8` |
| `modulo` | 剰余 (A % B) | 数値Aを数値Bで割った余り | `numA=10&numB=3` → `1` |

## 🌐 URL構築例

### 基本的な数値計算
```
# 10 + 5 = 15
https://yoursite.github.io/calculator/?numA=10&numB=5&operation=add

# 20 ÷ 4 = 5
https://yoursite.github.io/calculator/?numA=20&numB=4&operation=divide

# 2 ^ 8 = 256
https://yoursite.github.io/calculator/?numA=2&numB=8&operation=power
```

### テキスト処理込み
```
# 数値計算 + テキスト結合
https://yoursite.github.io/calculator/?numA=100&numB=50&operation=multiply&text1=Hello&text2=World&separator=%20

# 小数点計算
https://yoursite.github.io/calculator/?numA=10.5&numB=2.3&operation=add
```

### 特殊文字の扱い
```
# スペース区切り
separator=%20  (スペース)

# カンマ区切り  
separator=%2C  (カンマ)

# アンダースコア区切り
separator=_

# ハイフン区切り
separator=-
```

## 🎯 Smart Walk-Thrusでの実装例

### 1. 基本的な計算実行
```javascript
// WalkMe Smart Walk-Thru内
// Step 1: ブラウザアクション - URL遷移
const calculatorUrl = "https://yoursite.github.io/calculator/?numA=100&numB=25&operation=divide";

// Step 2: 要素待機
waitForElement('[data-walkme="math-result"]');

// Step 3: テキスト取得 → 変数保存
const result = getElementText('[data-walkme="math-result"]');
// 結果: "4" (100 ÷ 25)
```

### 2. 動的な値を使った計算
```javascript
// WalkMe変数を使用した動的URL構築
const userInput1 = getVariable('user_number_1'); // 例: 50
const userInput2 = getVariable('user_number_2'); // 例: 3
const calculatorUrl = `https://yoursite.github.io/calculator/?numA=${userInput1}&numB=${userInput2}&operation=multiply`;

// 結果取得: 50 × 3 = 150
```

### 3. 複数の処理を組み合わせ
```javascript
// 計算 + テキスト処理
const calculatorUrl = "https://yoursite.github.io/calculator/?numA=75&numB=25&operation=subtract&text1=Result&text2=Calculated&separator=:%20";

// 数値結果取得: 50 (75 - 25)
const mathResult = getElementText('[data-walkme="math-result"]');

// テキスト結果取得: "Result: Calculated"  
const textResult = getElementText('[data-walkme="text-result"]');
```

## 🔍 デバッグ用完全URL例

### 全パラメータ指定
```
https://yoursite.github.io/calculator/?numA=15.5&numB=4.2&operation=add&text1=WalkMe&text2=Smart&separator=_
```

**この場合の結果:**
- 数値計算: `19.7` (15.5 + 4.2)
- テキスト結合: `WalkMe_Smart`

### エラーケース確認用
```
# ゼロ除算エラー
https://yoursite.github.io/calculator/?numA=10&numB=0&operation=divide

# 無効な数値
https://yoursite.github.io/calculator/?numA=abc&numB=5&operation=add
```

## 📝 パラメータ省略時の動作

| パラメータ | デフォルト値 | 動作 |
|-----------|-------------|------|
| `numA` | なし | エラー表示 |
| `numB` | なし | エラー表示 |
| `operation` | `add` | 加算実行 |
| `text1` | 空文字 | 空文字として処理 |
| `text2` | 空文字 | 空文字として処理 |
| `separator` | ` ` (スペース) | スペース区切り |

## 🚀 実用的な活用例

### 割引計算
```
# 定価1000円の20%割引
https://yoursite.github.io/calculator/?numA=1000&numB=0.8&operation=multiply
# 結果: 800
```

### 税込み価格計算  
```
# 100円 × 1.1 (10%税込み)
https://yoursite.github.io/calculator/?numA=100&numB=1.1&operation=multiply
# 結果: 110
```

### 文字列整形
```
# ユーザー名 + ID結合
https://yoursite.github.io/calculator/?text1=UserName&text2=12345&separator=_
# 結果: UserName_12345
```
