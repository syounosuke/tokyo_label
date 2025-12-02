今後の構築フロー（ナカタニさん向け）
===================================

目的
- 「共通土台ガイド」をベースに、ページ別の『個別 土台指示書』に沿ってワイヤー（Snow Monkeyブロック）を構築し、伊藤さんがデザイン差し込み → 最終微調整へ繋ぐ。

前提（環境）
- WordPress（本番/検証）+ Snow Monkey 最新版
- 必須プラグイン: ACF, AIOSEO, Breadcrumb（任意: Breadcrumb NavXT 等）
- CPT/Tax: `ladies` + `status(active/archive)`、ACF: `db_profile_url` 他

1) 準備（1回のみ）
- Snow Monkey導入・基本設定（カラー/タイポは暫定でOK）
- AIOSEO 共通設定（タイトル/ディスクリプション/OGP既定、canonical方針）
- `WP-IMPLEMENTATION.md` のCPT/Tax/Rewrite/ACF/リダイレクトを子テーマに反映
- グローバルナビ作成（PC右上/モバイルはハンバーガー）

2) ページ骨組み（ワイヤー）作成
- 『個別 土台指示書』を確認
- ページ作成（固定ページ）
  - Heroは Coverブロック（90〜100vh, 中央/下中央）
  - ページ上部: ページタイトル（H1）+ 必要ならリード + パンくず
  - 本文: Section > Heading > Text > Button（Below段があれば追加）
  - 下層CTA: MEMBERS / RESERVE（ページ末尾、フッター直前）
  - フッターにCTAは置かない（自然導線のみ）
- 認証ページ（/）はナビ非表示、ENTER → /top

3) LADIES 一覧（active / archive）
- アーカイブを 2本に分割（/ladies, /ladies-archive）
- Query Loop でカード一覧を作成（画像/タイトル/タグ/要素）
- カードリンクは ACF `db_profile_url` へ（Fuwaly側に個別女性ページは作らない）

4) SEO/OGP/Schema（ワイヤー段階）
- AIOSEOでタイトル形式: {ページ名}｜TOKYO LABEL FUWALY
- ディスクリプション（共通ベース）設定、ページ別は後続で上書き
- canonicalは原則 `/` に集約（/top配下含む）、例外: 認証ページのみ `/` 固定
- OGPはブランド共通桜、認証のみ専用OGP
- 構造化データはテーマ/プラグインのどちらかに統一（重複禁止）。完全版は『ページ別 土台指示書』に準拠

5) デザイン差し込み（伊藤さん）
- 画像差替（Hero/下層背景/女性サムネ）
  - Hero: 認証=抽象（桜/光/白、人物なし）、トップ=女性5名 or 抽象光、下層=女性 or 抽象桜
  - 女性サムネ: 新規(800x1200), 旧(533x800)許容、WebP推奨
- 色/角丸/影/余白はトークン/追加CSSで反映（SCSS 上書き可）
- Heroは非lazy（その他はlazy可）

6) 最終調整（ナカタニさん）
- 余白/角丸/影/背景の微調整
- レスポンシブ/アクセシビリティ/パフォーマンス確認
- canonical/OGP/構造化データの最終確認（重複なし）

成果物チェックリスト
- [ ] 各ページのワイヤー（Snow Monkeyブロック）のみで過不足なし（無駄なブロック/クラス乱用なし）
- [ ] 認証ページはナビ非表示、ENTER→/top
- [ ] LADIESは一覧のみ（Singleは外部DBへ）
- [ ] 下層CTA（MEMBERS/RESERVE）はフッター直前に配置、フッターには置かない
- [ ] 画像仕様遵守（サイズ/形式、Hero非lazy）
- [ ] canonicalは/に集約（/top含む）、例外=認証のみ固定
- [ ] 構造化データは重複なし

補足
- 参考実装・コードは `prototype/WP-IMPLEMENTATION.md`, `prototype/AIOSEO-GUIDE.md` を参照。
