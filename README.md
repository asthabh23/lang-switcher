# Smart Language Switcher for AEM Edge Delivery Services

A multilingual website implementation with intelligent URL mapping between English and French languages using AEM Edge Delivery Services.

## Features

- **Automatic Language Detection**: Detects current language from URL path
- **Intelligent URL Mapping**: Maps pages between languages using placeholders.json configuration
- **Seamless Navigation**: One-click language switching in the header
- **Fallback Support**: Graceful fallbacks when mappings are unavailable
- **Homepage Handling**: Special handling for homepage language switching (`/` ↔ `/fr`)

## Environments
- Preview: https://main--lang-switcher--asthabh23.aem.page/
- Live: https://main--lang-switcher--asthabh23.aem.live/


## Language Switcher Configuration

### URL Structure
- **English (default)**: `/` or `/en/page-path`
- **French**: `/fr` or `/fr/page-path`

### Placeholders Setup

Create a `placeholders` sheet at the root of your content with a `language-switcher` tab consisting of URL mappings between languages as:


### How URL Mapping Works

1. **Current page detection**: Extracts the current language and page path
2. **Mapping lookup**: Searches placeholders.json for the current page mapping
3. **Target URL construction**: Builds the target language URL using the mapped path
4. **Fallback handling**: Uses standard language prefix pattern if no mapping found

Example mappings:
- `/en/example-content` → `/fr/contenu-d-example`
- `/en/getting-started/guide` → `/fr/prise-en-main/guide-rapide`
- `/en//getting-started/documentation` → `/fr/prise-en-main/documentation`
- `/` → `/fr` (homepage special case)

## Implementation Details

### Header Integration

The language switcher is automatically integrated into the header navigation by detecting clickable elements in the nav-tools section and attaching event listeners. The implementation can be found in [`blocks/header/header.js`](blocks/header/header.js) - look for the "Initialize language switcher" section.

### Language Detection

The system automatically detects the current language from the URL path using the `getLanguage()` function. This logic is implemented in [`scripts/scripts.js`](scripts/scripts.js) and handles the URL parsing and language detection.

### URL Mapping Logic

The core language switching functionality is implemented in [`scripts/language-switcher.js`](scripts/language-switcher.js):

1. **Fetch mappings** from `placeholders.json?sheet=language-switcher` - see `fetchLanguageMappings()`
2. **Find current page** mapping by matching the current language URL - see `findMappedUrl()`
3. **Switch to target** language using mapped URL or fallback pattern - see `switchToLanguage()` and `switchLanguage()`

## Usage

### Adding the Language Switcher on the header of the site for easy access (can be extended for more languages as needed)

The language switcher is located in the **header of the site** and appears as a **blue highlighted clickable element**.


### Creating Language Mappings

1. Create or update `placeholders.json` in your site root (at content level)
2. Add entries to map between English and French URLs
3. Use empty strings for homepage mapping
4. Preview/publish changes to see them take effect

### Supported Languages

- `en` (English) - Default language
- `fr` (French) - Secondary language

## File Structure

```
├── blocks/header/header.js          # Language switcher integration
├── scripts/language-switcher.js     # Core language switching logic
├── scripts/scripts.js              # Language detection utilities
└── README.md                       
```

## Troubleshooting

- **Language switcher not working**: Check that nav-tools contains a `<p>` element
- **Incorrect redirects**: Verify placeholders.json mapping configuration
- **404 errors**: Ensure target language pages exist at mapped URLs
- **Fallback not working**: Check that both languages are defined in LANGUAGES constant
