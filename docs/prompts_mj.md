# Midjourney 運用メモ

華子の表情差分を生成・再生成するための手順。

## 使用する機能

Midjourney V8系の **Edit model (Attach to prompt)**。

Omni Reference は廃止済み。過去記事や過去のプロンプトに `--oref` が出てきても使わない。

## 鉄則: 常に 01 を参照元にする

毎回 `hanako_01_normal.png` を参照元として添付する。

**差分から派生させない。**
`02` を元に `03` を作り、`03` を元に `04` を作る、という連鎖をやると、
一枚ごとの微差が累積して顔が徐々にずれる。
3枚も進むと別人になり、しかも途中のどこでずれたかが分からなくなって全部やり直しになる。

必ず 01 → 02、01 → 03、01 → 04、と放射状に作る。

## プロンプトに必ず入れる語

```
same woman
same dark brown eyes
```

`same dark brown eyes` は必須。
これを省くと**瞳の色が青灰色に変わる事故が実際に起きた**。
一度起きているので、省略できる指定だと思わないこと。

## 否定形で書く

`neutral` は弱い。ほぼ効かない。
表情を抑えたいときは否定形で指定する。

| ダメ | 書き方 |
|---|---|
| `neutral expression` | `not smiling` |
| `calm eyes` | `eyes not smiling` |
| `tired` | `not smiling, dark circles under eyes` |

Midjourney は肯定形の抽象語をほぼ無視して、学習分布の平均（＝微笑み）に寄る。
「〜でない」と書いたときだけ平均から引き剥がせる。

## 固定して書く要素

差分間でぶれさせたくない要素は、毎回同じ文言で書く。省略しない。

```
late twenties japanese woman,
black hair tied back, slightly messy after work,
grey shirt,
her room at night, warm lamp light,
photorealistic
```

## テンプレート

```
[hanako_01_normal.png を添付]

same woman, same dark brown eyes,
late twenties japanese woman, black hair tied back, slightly messy after work,
grey shirt, her room at night, warm lamp light, photorealistic,
<<< ここに表情の指定を否定形中心で書く >>>
```

## 表情ごとの指定

| face | 指定 |
|---|---|
| `02` tired | `not smiling, dark circles under eyes, slouching posture, shoulders dropped` |
| `03` limit | `not smiling, eyes unfocused, looking at nothing in front of the screen, blank face` |
| `04` hurt | `hurt expression, eyes wide, holding back tears, not crying yet` |
| `05` awkward | `not smiling, looking away from camera, avoiding eye contact, uncomfortable` |
| `06` smile | `genuine smile, eyes smiling, relaxed shoulders, warm expression` |

`06` だけは否定形を使わない。ここは本当に笑わせたい唯一の差分なので、
Midjourney が平均に寄ること自体が有利に働く。

## 生成後のチェック

採用前に `hanako_01_normal.png` と並べて確認する。

- [ ] 瞳が濃い茶色のままか（**最優先**。青灰色化の実績あり）
- [ ] 髪の結び方と崩れ方が同じか
- [ ] シャツがグレーのままか
- [ ] 光源が同じ側から来ているか
- [ ] 顔の造作が同一人物に見えるか（並べて見ないと分からない）

一つでも外れたら採用しない。再生成する。
「この差分だけ少し違うが許容範囲」を一枚許すと、次の一枚の基準がそこに引きずられる。
