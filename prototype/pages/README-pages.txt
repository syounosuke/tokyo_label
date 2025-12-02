FUWALY 共通土台（静的モック）ページ構成
=====================================

目的
- 「共通土台ガイド」に沿って、全ページ共通のレイアウト・余白・見出し階層・アクセシビリティ基盤をそろえた静的モックを提供します。
- 色・画像・微調整は「デザイン総合ガイド」で統一予定。ここでは骨組み（Header/Section/Footer/パンくず/見出し）を固定します。

ページ一覧（全11）
1. 認証（root・ナビ非表示）: ../index.html
2. CONCEPT: concept.html
3. 特徴: features.html
4. ラインナップ（= LADIES想定）: lineup.html
5. MEMBERS: members.html
6. 商品詳細（サンプル）: product.html
7. お知らせ一覧: news.html
8. お知らせ詳細（サンプル）: news-single.html
9. よくあるご質問（FAQ）: faq.html
10. お問い合わせ: contact.html
11. プライバシーポリシー（POLICY）: privacy.html
12. 利用規約: terms.html
補足: TOPページは `../top/index.html` に配置（/top 配下）。

補助ページ（任意追加例）
- SYSTEM: system.html
- RESERVE: reserve.html
- RECRUIT: recruit.html
- COMPANY: company.html
- 取扱店舗: stores.html
- ブランドガイド: guide.html
- 資料ダウンロード: downloads.html

共通ルール
- Header: PCは右上配置、SPはハンバーガー（縦並び）。
- グローバルナビ: TOP / CONCEPT / LADIES / SYSTEM / FAQ / RESERVE / RECRUIT / COMPANY（認証ページでは表示しない）
- ナビ色: 白ベース（SCSSで上書き可）。
- 該当ページは aria-current="page" 指定。
- ページ上部: .page-hero に H1（1ページ1つ）と必要に応じてリード文、パンくず .breadcrumb。
- セクション余白: .s-tight / .s-normal / .s-loose を使用（Snow Monkey Section の余白に対応）。
- 見出し階層: H1（ページタイトル）> H2（セクション）> H3（カードや項目）。
- 文字/コントラスト/フォーカス: :focus-visible を有効化、本文コントラストを確保。
- フッター: 3カラム（ブランド/メニュー/サポート）。規約・プライバシーはサポートへ配置。

Snow Monkey 実装メモ
- Heroは Coverブロックで構築（90〜100vh、中央または下中央配置、背景画像はデザインガイド準拠。認証ページは女性写真なしの抽象背景）。
- Header/Navigation は外観メニューで再現。CTAはボタンスタイル。
- .page-hero は Snow Monkey Section + Page title ブロックで代替。
- パンくずはプラグイン/テーマ機能を使用（例: Breadcrumb NavXT）。
- 内部導線: 重要CTAは MEMBERS / RESERVE（Heroやページ末尾CTAで優先的に配置）。
- カード/グリッドは Columns/Group + 画像/見出し/段落の組合せ、または Items/Posts ブロックで代替。
- 余白は Section ブロックのマージン/パディングプリセットで再現、足りない場合は追加CSS。

次のステップ
- デザイン総合ガイドの確定値（カラー/画像/余白）を tokens に反映（assets/styles.css）。
- ページ別の具体指示に従って、各ページの中身を Snow Monkey のブロックで置換・詳細化。
