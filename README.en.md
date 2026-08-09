*[한국어](README.md) · English*

# boxwine-content

Learning content for **BoxWine**, a wine tasting note app.

The content is **not bundled into the APK**. The app fetches it from this repository's raw
URLs at runtime and caches it locally, so entries and images can be added or changed
**without rebuilding the app**.

## Files

| File | Purpose |
|---|---|
| `wine_glossary.json` | The content itself. Currently `_meta.version` **6**, 151 entries |
| `images/<category>/*.jpg` | Header image per entry. Currently **145** images |
| `wine_names_ko.json` | Korean-transliteration dictionary for wine names. ~1MB, so it is remote-only and never bundled |

Categories in `wine_glossary.json` — grape varieties 34 · tasting terms 24 · regions 39 ·
aroma/flavor groups 5 · wine styles 11 · classifications 11 · winemaking 3 · producers 24.

URLs the app reads:

- `https://raw.githubusercontent.com/binbox01/boxwine-content/main/wine_glossary.json`
- `https://raw.githubusercontent.com/binbox01/boxwine-content/main/wine_names_ko.json`
- Images resolve the JSON's relative `image` path against `images/` next to that JSON

## Updating

Edit the content **in the app repository under `assets/content/`**, then push it here with
the sync script. Editing this repository directly gets overwritten on the next sync
(images are mirrored with `robocopy /MIR`).

```powershell
# run from the app repository root

# copy only, then review what changed
powershell -File scripts/sync_content_repo.ps1

# copy, commit and push
powershell -File scripts/sync_content_repo.ps1 -Push -Message "content: add 3 grape varieties (v7)"
```

⚠️ Whenever the content changes, **bump `_meta.version` in `wine_glossary.json` by 1**.
The app only refreshes when this value is greater than its cached version — without the
bump, the change never reaches the app.

## Image licensing

Photographs come only from Wikimedia Commons under **CC BY · CC BY-SA · CC0 · PD**, and
every entry records source, author and license in `imageCredit` (135 images).

Concepts that cannot exist as a photograph (such as the aroma/flavor group still lifes) are
AI-generated and declare `source` as `AI 생성` ("AI generated"); `url` and `license` are left
empty because there is no original page or license (10 images). **Real regions and producers
are never AI-generated** — that would invent a place that does not exist, and it would be
indistinguishable from a photograph.
