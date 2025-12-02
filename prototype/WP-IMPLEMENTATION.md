Implementation Notes (WordPress + Snow Monkey)
==============================================

CMS: WordPress
Theme: Snow Monkey (latest)
Build: Fixed pages base + CPT `ladies`

1) Pages (固定ページ)
- Build each static page in Gutenberg using the provided JSON blocks or the prototype as reference.
- Under `/top` on production, set canonical to `/` for all child pages to aggregate SEO to root.
  - With Yoast SEO: add a filter to override the canonical.
  - Without Yoast: use `rel_canonical` filter.

Sample (Yoast): in `functions.php`
```php
add_filter('wpseo_canonical', function ($canonical) {
  if (is_page() && strpos($_SERVER['REQUEST_URI'] ?? '', '/top/') === 0) {
    return home_url('/');
  }
  return $canonical;
});
```

Core canonical filter alternative:
```php
add_filter('rel_canonical', function ($canonical) {
  if (is_page() && strpos($_SERVER['REQUEST_URI'] ?? '', '/top/') === 0) {
    return home_url('/');
  }
  return $canonical;
});
```

2) CPT: `ladies`（ACTIVE/ARCHIVE）
- Register a custom post type for the listing. Keep archive enabled; single pages redirect out.
```php
add_action('init', function () {
  register_post_type('ladies', [
    'labels' => [
      'name' => 'Ladies',
      'singular_name' => 'Lady',
    ],
    'public' => true,
    'has_archive' => true,
    'rewrite' => ['slug' => 'ladies'],
    'show_in_rest' => true,
    'supports' => ['title','editor','thumbnail','excerpt'],
    'menu_position' => 20,
  ]);

  // Taxonomy: status (active / archive)
  register_taxonomy('status', 'ladies', [
    'labels' => ['name' => 'Status'],
    'public' => true,
    'show_in_rest' => true,
    'hierarchical' => false,
    'rewrite' => false, // custom rewrite below to get /ladies and /ladies-archive
  ]);

  // Ensure terms exist
  foreach (['active','archive'] as $slug) {
    if (!term_exists($slug, 'status')) {
      wp_insert_term(ucfirst($slug), 'status', ['slug' => $slug]);
    }
  }

  // Add rewrite tags and rules to split archives:
  add_rewrite_tag('%ladies_state%', '([^&]+)');
  add_rewrite_rule('^ladies/?$', 'index.php?post_type=ladies&ladies_state=active', 'top');
  add_rewrite_rule('^ladies-archive/?$', 'index.php?post_type=ladies&ladies_state=archive', 'top');
});
```

3) ACF: `db_profile_url`
- Create a URL field on `ladies` named `db_profile_url`.
- On single lady, redirect to the external DB profile.
```php
add_action('template_redirect', function () {
  if (is_singular('ladies')) {
    $url = function_exists('get_field') ? (string) get_field('db_profile_url') : '';
    if ($url) {
      wp_redirect($url, 301);
      exit;
    }
  }
});

// Filter archives by taxonomy based on custom query var from rewrite
add_action('pre_get_posts', function ($q) {
  if (!is_admin() && $q->is_main_query() && $q->is_post_type_archive('ladies')) {
    $state = get_query_var('ladies_state');
    if ($state === 'active' || $state === 'archive') {
      $tax_query = [
        [
          'taxonomy' => 'status',
          'field'    => 'slug',
          'terms'    => $state,
        ],
      ];
      $q->set('tax_query', $tax_query);
    }
  }
});

// Flush rewrite on theme activation or plugin activation (one-time)
// register_activation_hook(__FILE__, function(){ flush_rewrite_rules(); });
// add_action('after_switch_theme', function(){ flush_rewrite_rules(); });
```

ACF fields for `ladies`
- age (text or number)
- height (text or number)
- size_bwh (text) e.g. "B-W-H"
- job (text)
- tag_1 (text)
- tag_2 (text)
- tag_3 (text)
- thumbnail_main1 (image) — target size 800x1200 (WebP preferred)
- db_profile_url (url) — external profile URL (required)

Card linking (archive/list)
- Fuwaly側に個別ページは作らず、一覧カードは DB個別ページへ遷移。
- Use this to override links in loops to the external URL:
```php
add_filter('post_type_link', function ($permalink, $post) {
  if ($post->post_type === 'ladies') {
    $url = function_exists('get_field') ? (string) get_field('db_profile_url', $post->ID) : '';
    if ($url) return $url;
  }
  return $permalink;
}, 10, 2);
```

Notes:
- Fuwaly側は一覧のみ保持 → Use the archive (`/ladies/`) and any taxonomy filters; avoid exposing single content by redirecting.
- If you must show a bridge page, render a CTA button to the external URL and set canonical to the external profile; otherwise 301 redirect is cleaner.

4) Archive template (Snow Monkey blocks)
- Create a page "Ladies" (optional) and set the CPT archive to use Query Loop filtering `post type = ladies` OR let the native archive render and build via block theme parts if using FSE.
- Fields displayed in list: title, thumbnail, taxonomy terms (if any), short excerpt, and a button "プロフィールを見る" linking to `db_profile_url` (open new tab or same tab per policy).

5) Global Navigation（全ページ共通）
- Configure Appearance > Menus (or Site Editor > Navigation) for the global items.
- Mark the current menu with `aria-current`. For CTA, use Button style.

CTA buttons
- Use Snow Monkey standard buttons (Buttons/Button blocks).
- Corner radius is applied via CSS (see assets/styles.css: `.wp-block-button .wp-block-button__link { border-radius: var(--radius-md) }`).
- Layout: PC 2つ横並び、SP 縦積み（CSSで `.cta` セクション内の Buttons を縦方向に）。

Internal linking priority
- 重要CTAは MEMBERS / RESERVE。
- Hero / セクション末尾 / ページ末尾のCTAで、MEMBERS/RESERVE を優先配置。

6) Auth root domain
- 評価集約ドメイン: https://aoyama-fuwaly.com/ （認証ページ＝root）
- Ensure canonical across `/top/*` points to `/`. Consider disallowing `/top/` in robots or keeping indexable with canonical depending on your policy.

7) Performance/Accessibility
- Use core image sizes and `loading="lazy"`.
- Keep color contrast ≥ 4.5:1 for text on backgrounds.
- Enable skip link and focus outlines (provided in CSS).

Images spec (unified)
- Hero backgrounds:
  - Auth (/): abstract (sakura/light/white). No people photos.
  - Top: 5 women or abstract light (per guide).
  - Lower pages: women or abstract sakura as instructed per page.
- Ladies thumbnails:
  - New (Active): 800x1200 portrait (WebP recommended)
  - Legacy (Archive): 533x800 allowed
  - Register sizes:
```php
add_action('after_setup_theme', function(){
  add_image_size('lady_portrait', 800, 1200, true);
  add_image_size('lady_portrait_legacy', 533, 800, true);
});
```
- Lazy loading: allowed sitewide, but not on Hero.
  - For Top cover image, set eager loading / high priority:
```php
add_filter('wp_get_attachment_image_attributes', function($attr, $attachment){
  if ((is_front_page() || is_page('top')) && isset($attr['class']) && strpos($attr['class'], 'wp-image') !== false) {
    $attr['loading'] = 'eager';
    $attr['fetchpriority'] = 'high';
  }
  return $attr;
}, 10, 2);
```

8) Deployment checklist
- [ ] Register CPT and ACF field.
- [ ] Add canonical filter for `/top/*`.
- [ ] Build pages from patterns.
- [ ] Create Navigation menu.
- [ ] Configure archive ladies listing.
- [ ] Verify single ladies redirect.

9) 禁止事項（実装上の注意）
- 個別女性ページをFuwaly側に作らない（Singleは必ず外部DBへリダイレクト）
- トップを canonical /top にしない（必ず / に集約）
- 構造化データの二重設定を避ける（テーマ/プラグインいずれかに統一。AIOSEOのSchemaを使う場合はテーマ側の出力を抑制）
- 無駄なブロック追加禁止（Section/Heading/Text/Button の基本構造を遵守）
- classの乱用禁止（必要最小限。見出し階層・余白はプリセット優先）
- Heroに重い動画禁止、パララックスJSアニメーション禁止（パフォーマンス・可読性優先）
```
