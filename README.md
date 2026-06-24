# ACF Relationship Dynamic Widget

A production-ready Elementor widget that automatically displays related posts via ACF Relationship fields — with multiple layout modes, taxonomy filtering, AJAX pagination, bidirectional relationships, and a theme-overridable template system.

---

## Requirements

| Dependency | Minimum Version |
|---|---|
| WordPress | 6.0+ |
| PHP | 8.0+ |
| Elementor (free) | 3.x+ |
| Advanced Custom Fields (ACF) | 5.x+ |
| Elementor Pro *(optional)* | For Loop Template integration |

---

## Installation

1. Upload the `acf-relationship-widget/` folder to `wp-content/plugins/`.
2. Activate via **Plugins → Installed Plugins**.
3. The widget appears in the Elementor panel under the **General** category as **"ACF Relationship"**.

---

## Quick Start

1. Create an ACF Relationship field (e.g. `team_members`) attached to a post type.
2. Drag the **ACF Relationship** widget onto an Elementor page template.
3. In the widget panel → **Data Source**, enter `team_members` as the field name.
4. Choose a layout, set column count, and publish.

---

## Widget Controls Reference

### Data Source
| Setting | Description |
|---|---|
| ACF Field Name | The exact ACF field key attached to the current post (e.g. `team_members`). |
| Empty State | `Hide widget` or `Show fallback message` when field is empty. |
| Fallback Message | Text shown when empty state is set to *message*. |

### Layout
| Setting | Description |
|---|---|
| Layout | Grid / Carousel / List / Table |
| Columns | Number of columns (responsive: desktop / tablet / mobile). |
| Item Gap | Spacing between cards (px). |
| Carousel Autoplay | Enable auto-advance in carousel mode. |
| Autoplay Delay | Milliseconds between slides. |

### Query
| Setting | Description |
|---|---|
| Max Items | Maximum posts to show (initial load). |
| Order By | ACF Selection Order / Date / Title |
| Order | ASC / DESC |
| Taxonomy | Taxonomy slug to filter related posts (e.g. `category`). |
| Term Slugs | Comma-separated term slugs (e.g. `design, development`). |
| Operator | IN / NOT IN / AND |

### Card / Template
| Setting | Description |
|---|---|
| Show Featured Image | Toggle image display. |
| Image Size | Any registered image size. |
| Show Title | Toggle title. |
| Title HTML Tag | h2–h5 or p. |
| Show Excerpt | Toggle excerpt display. |
| Excerpt Words | Word count for trimmed excerpt. |
| Extra ACF Fields | Comma-separated field names from the *related* post to show in card meta (e.g. `position, department`). |
| Link Target | `_self` (same tab) or `_blank` (new tab). |
| Elementor Loop Template ID | (Pro only) Enter a Loop Template post ID to render each card using that template instead of the PHP partial. |

### Advanced Features
| Setting | Description |
|---|---|
| Bidirectional Relationship | Also fetch posts that reference the current post via the specified reverse field. |
| Reverse Field Name | The ACF field on the *related* post that points back to this post. |
| AJAX Load More | Appends more items on button click without a page reload. |
| Button Text | Custom label for the load-more button. |

---

## Theme Template Overrides

All four card templates can be overridden from your theme. Copy the relevant file and place it here:

```
your-theme/
└── acf-rel-widget/
    ├── card-grid.php
    ├── card-carousel.php
    ├── card-list.php
    └── card-table.php
```

Variables available inside each template:

| Variable | Type | Description |
|---|---|---|
| `$post` | `WP_Post` | The related post object. |
| `$settings` | `array` | Full Elementor widget settings. |
| `$extra_fields` | `string` | Comma-separated extra ACF field names. |

---

## Elementor Loop Template (Pro)

1. In Elementor Pro, create a **Loop Item** template under **Templates → Theme Builder → Loop Item**.
2. Design your card using dynamic tags.
3. Publish and note the **template post ID** (visible in the URL bar).
4. In the widget panel → **Card / Template**, paste the ID into **Elementor Loop Template ID**.

The PHP template is used as a fallback if Elementor Pro is not active.

---

## Bidirectional Relationships

Scenario: *Post A* has field `related_projects` pointing to *Post B*. You want *Post B*'s page to also show *Post A* as related.

1. On *Post B*'s template, place the widget.
2. Set **ACF Field Name** to `related_projects`.
3. Enable **Bidirectional Relationship**.
4. Set **Reverse Field Name** to `related_projects` (or a different field if applicable).

The query will union both directions automatically.

---

## AJAX Load More

Enable **AJAX Load More** in the Advanced Features section. A "Load More" button is appended below the grid/list/carousel. Clicking it fetches the next batch (size = **Max Items**) and appends them in place. The button disappears once all related posts are loaded.

---

## Filters & Hooks

```php
// Modify the WP_Query args before execution.
add_filter( 'acf_rel_widget_query_args', function( array $args ): array {
    // e.g. restrict to a specific post type.
    $args['post_type'] = 'team_member';
    return $args;
} );

// Modify the rendered HTML of a single card.
add_filter( 'acf_rel_widget_card_html', function( string $html, WP_Post $post, array $settings ): string {
    return $html;
}, 10, 3 );
```

*(Hooks are placed in `Query_Builder::get_related()` and `Template_Loader::render_php_card()` — add `apply_filters()` calls there as needed.)*

---

## Changelog

### 1.0.0
- Initial release.
- Grid, carousel (Swiper), list, table layouts.
- ACF Relationship field support (post objects or IDs).
- Bidirectional relationship queries.
- Taxonomy filtering.
- AJAX Load More pagination.
- Responsive column control.
- Elementor Loop Template integration (Pro).
- Theme-overridable PHP card templates.
- Elementor style controls for card background, radius, shadow, typography.
