# CLAUDE.md — new-office-staging

> **適用範囲 / Scope**: `/Users/tokyoaibos/Desktop/AIBOS/new-office-staging/` で作業する全セッションで有効。
> 新オフィス内装解体の進捗を社内・投資家向けに共有する GitHub Pages サイトの **ステージング環境**。

---

## 1. 役割 / Role

- **このリポジトリはステージング（テスト環境）**。本番は `/Users/tokyoaibos/Desktop/AIBOS/new-office/` にある別リポジトリ。
- 新機能・デザイン変更・バグ修正は **必ずこちらで先に試す**。モバイル実機で問題がなければ本番にコピーする。
- This is the **staging repo**. Production is a separate repo at `/Users/tokyoaibos/Desktop/AIBOS/new-office/`. All changes go here first; copy to production only after mobile verification.

---

## 2. デプロイ / Deploy

| 項目 / Item | 値 / Value |
|---|---|
| Remote | `github.com/YuichiCivic/new-office-staging` |
| Branch | `main`（直接 push） |
| Build type | `legacy` (GitHub Pages, `main` 直下) |
| 公開 URL / URL | https://yuichicivic.github.io/new-office-staging/ |
| 反映 / Propagation | push 後 1〜3 分 |

`main` に push すると自動デプロイされる。PR フローは使っていない（1人運用）。

---

## 3. 主要ファイル / Key File

**`index.html` のみ** — single-file HTML（inline CSS / JS、外部依存は Google Fonts の Inter のみ）。

- **テーマ**: Apple 風。ダーク / ライトモード切替（localStorage 記憶）
- **言語**: JA / EN 切替。初回は `navigator.language` で自動判定。バイリンガル併記ではなく選択式
- **ロゴ**: base64 埋め込み PNG。`mix-blend-mode: screen`（ダーク） / `multiply`（ライト）で白枠を消す
- **Day カード**: 降順表示（最新が上）。ステータスバッジ 完了=緑 / 予定=オレンジ
- **写真ライトボックス**: タップでサイト内ビューア起動。スワイプでナビ、長押しで保存
- **ソース of truth**: `../New building/work_log.md` の内容を反映する形で更新する

---

## 4. ワークフロー / Workflow

日次更新は skill `/new-building-reporting` で自動化されている。手動で編集する場合:

1. **編集**: `index.html` を修正
2. **ローカル確認**: `open index.html` でブラウザ確認
3. **commit**: `feat:` / `fix:` / `docs:` のいずれかで日本語コミット
4. **push**: `git push origin main` → GitHub Pages 自動ビルド
5. **モバイル実機確認**: 公開 URL をスマホで開く
6. **本番反映**: 問題なければ `cp index.html ../new-office/index.html` して本番リポジトリでも commit & push

---

## 5. よくある落とし穴 / Known Pitfalls

- **スプラッシュスクリーン**: 過去に何度もモバイルで黒画面バグの原因になった。現状は animation ベースで動作中だが、触る時は必ずモバイル実機で確認する
- **`applyClasses()` は `document.body.className` を完全に上書きする**。splash など body 以外の要素の class を消すことはないが、body に他の class を足す時は注意
- **`@import` の Google Fonts**: render-blocking になり得る。ネットワーク遅い環境ではスプラッシュ表示が長引く可能性
- **ライトボックスのスワイプ**: `touchstart/move/end` を `lightbox` 要素にバインド（track だとスライド先で touch を失う）。`justSwiped` フラグで backdrop click の誤発火を防止

---

## 6. 関連 / Related

- **本番**: `/Users/tokyoaibos/Desktop/AIBOS/new-office/` (`github.com/YuichiCivic/new-office`)
- **作業記録**: `/Users/tokyoaibos/Desktop/AIBOS/New building/work_log.md`
- **Skill**: `/new-building-reporting`（プランニング・記録・写真追加・サイト更新の一括処理）
- **Memory**: `project_renovation_workflow.md`, `reference_renovation.md`
- **Google Drive（動画）**: https://drive.google.com/drive/folders/1duijNaTXd5vJ5asypoz6OFZdqqK4866A

---

## 7. 言語ポリシー / Language Policy

- ワークスペース共通ルール: バイリンガル（日本語先行 → 英語）
- index.html 内は **JA/EN 切替式**（バイリンガル併記ではない）
- コミットメッセージは日本語・英語どちらでも可（1行目 72 文字以内）
- コード内のコメント・変数名は英語
