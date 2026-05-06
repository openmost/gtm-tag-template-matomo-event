# Matomo Event — Google Tag Manager template

A Google Tag Manager community template that sends a custom **event** to Matomo through the standard `_paq` queue, with optional event value and custom dimensions attached to the event.

Authored by Ronan HELLO — [Openmost](https://openmost.com).

---

## What this tag does

When the tag fires, it pushes a `trackEvent` call to the Matomo tracker:

```js
_paq.push(['trackEvent', eventCategory, eventAction, eventName, eventValue, customDimensions]);
```

This is the standard Matomo Event Tracking call, exposed as a configurable GTM tag so you don't have to write Custom HTML or Custom JavaScript.

Typical use cases:

- Outbound link clicks
- Form submissions
- Video plays / pauses / completions
- Add-to-cart, wishlist, share
- Any user interaction you want to report on in Matomo's *Behaviour → Events* report

---

## Prerequisites

1. A working **Matomo** instance (self-hosted or Matomo Cloud).
2. A **Google Tag Manager** container loaded on your site.
3. The Matomo base tracker (`_paq`) must already be initialised on the page — typically via the official Matomo Analytics tag or a Custom HTML tag that bootstraps `_paq` and loads `matomo.js`.

This template only **adds events to the `_paq` queue**; it does not load the Matomo tracker itself.

---

## Installation

### Recommended — install from the Community Template Gallery

This is the easiest path and gives you automatic update notifications when a new version is published.

1. In GTM, open your workspace and go to **Templates → Tag Templates → Search Gallery**.
2. Search for **"Matomo Event"** by **Openmost**.
3. Click **Add to workspace** and accept the requested permissions.

That's it — the tag type **Matomo Event** is now available when you create a new tag.

### Alternative — import `template.tpl` manually

Use this only if you can't access the Community Gallery (e.g. private GTM environment) or want to fork / customise the template.

1. In GTM, go to **Templates → Tag Templates → New**.
2. Open the menu (⋮) → **Import**.
3. Select `template.tpl` from this repository.
4. Save.

---

## Configuration

Once the template is added, create a new tag of type **Matomo Event** and fill in the fields below.

| Field | Required | Description |
|---|---|---|
| **Event category** | Yes | Top-level grouping for the event (e.g. `Video`, `Form`, `Outbound link`). |
| **Event action** | Yes | The interaction that occurred (e.g. `Play`, `Submit`, `Click`). |
| **Event name** | Yes | A label that gives more context (e.g. video title, form name, target URL). |
| **Event value** | No | A numeric value associated with the event (e.g. price, score, duration in seconds). |
| **Custom dimensions** | No | A table of `Dimension ID` / `Dimension value` pairs. The Dimension ID must be a positive integer matching a custom dimension configured in Matomo with the **Action** scope. |

### Tip — using GTM variables

Every field accepts GTM variables (e.g. `{{Click URL}}`, `{{DLV - product_name}}`), so you can build dynamic events from the dataLayer.

---

## Triggering

Attach the tag to any GTM trigger that represents the user action you want to record — *Click - Just Links*, *Form Submission*, a *Custom Event* pushed from the dataLayer, etc.

> **Important:** make sure the Matomo base tag has fired **before or with** this event tag (use *Tag Sequencing* or rely on the same trigger), otherwise `_paq` may not yet exist.

---

## Permissions

The template requests one permission:

- **Access global variables → `_paq`** (read / write / execute) — required to push events into Matomo's tracker queue.

No external network requests are made directly by the template; the actual HTTP call to Matomo is performed by the Matomo tracker library (`matomo.js`).

---

## Custom dimensions — how the data is sent

When you fill in the **Custom dimensions** table, the template builds a dimensions object of the shape Matomo expects:

```js
{ dimension1: 'value1', dimension5: 'value2' }
```

…and passes it as the 5th argument of `trackEvent`. Only dimensions with the **Action** scope are recorded against the event in Matomo.

---

## Troubleshooting

- **Nothing appears in Matomo** — open Matomo's *Visitors → Visits Log* in real-time mode and check the GTM *Preview* mode to confirm the tag fired.
- **`_paq is not defined` warning** — the Matomo base tracker has not loaded yet. Adjust your trigger or use Tag Sequencing.
- **Custom dimension not recorded** — confirm the dimension exists in Matomo with scope **Action**, and that its ID is a positive integer.

---

## License

See [LICENSE](LICENSE).
