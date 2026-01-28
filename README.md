# ZeroClick Global Click Tracking - Google Tag Manager Template

Capture ZeroClick attribution data on your website using Google Tag Manager. This tag stores the ZeroClick visitor identifier as a first-party cookie for conversion attribution.

## Overview

The ZeroClick Global Click Tracking tag is a custom Google Tag Manager template that captures the ZeroClick visitor identifier (`zcId`) when a user arrives on your site from a ZeroClick ad. It stores the identifier as a first-party cookie and in localStorage so it's available later on your conversion page.

This tag is the **first step** in ZeroClick conversion tracking. It works together with the [ZeroClick Conversion Tag](https://github.com/piedotorg/zeroclick-conversion-gtm-tpl) to attribute conversions to specific ZeroClick ad clicks.

## Features

- **Zero Configuration**: No parameters required -- just install and it works
- **Automatic Detection**: Captures `zcId` from URL query parameters when users arrive from ZeroClick ads
- **First-Party Cookie Storage**: Stores the visitor ID as a first-party cookie (`_zcId`, 30-day expiry)
- **localStorage Backup**: Also stores in localStorage as a fallback
- **ITP Compatible**: Uses first-party cookies, so it works in all modern browsers regardless of third-party cookie restrictions
- **Lightweight**: Minimal code, no external dependencies

## Installation

### 1. Import the Template to GTM

1. Download [template.tpl](template.tpl) from this repository
2. In Google Tag Manager, navigate to **Templates** > **Tag Templates**
3. Click **New** > **Import**
4. Select the downloaded `template.tpl` file
5. Click **Save**

### 2. Create a New Tag

1. Navigate to **Tags** > **New**
2. Click **Tag Configuration**
3. Select **ZeroClick global click tracking** from the Custom templates section
4. No configuration needed -- the tag has no parameters
5. Set up a trigger to fire on **All Pages**
6. Save and publish your container

## Configuration

This tag requires **no configuration**. It automatically detects the `zcId` query parameter and stores it.

## How It Works

1. **User clicks a ZeroClick ad** and is redirected to your site with `?zcId=xxx` in the URL
2. **This tag fires on page load**, reads the `zcId` from the URL query parameters
3. **Stores the `zcId`** in two places:
   - First-party cookie `_zcId` (30-day expiry, secure, SameSite=Lax)
   - localStorage key `_zcId`
4. **Later, on the conversion page**, the [ZeroClick Conversion Tag](https://github.com/piedotorg/zeroclick-conversion-gtm-tpl) reads the stored `_zcId` and sends it with the conversion event

```
User clicks ZeroClick ad
      |
      v
Redirected to your site with ?zcId=xxx
      |
      v
Global tag captures zcId, stores as first-party cookie (_zcId, 30 days)
      |
      v
User browses your site, adds to cart, checks out
      |
      v
On thank-you page, conversion tag reads _zcId from cookie
      |
      v
Fires conversion request to ZeroClick with both pixelId and zcId
      |
      v
ZeroClick attributes the conversion to the original ad click
```

## Two-Tag Setup

For full ZeroClick conversion tracking, you need **two tags** in GTM:

| Tag | Trigger | Purpose |
|-----|---------|---------|
| **ZeroClick Global Click Tracking** (this tag) | All Pages | Captures visitor identity on landing |
| **[ZeroClick Conversion Tag](https://github.com/piedotorg/zeroclick-conversion-gtm-tpl)** | Conversion page only | Fires the conversion event |

Both tags are required for attributed conversions. Without this global tag, conversions are still tracked but cannot be tied back to a specific ad click.

## Testing

### GTM Preview Mode

1. Enable **Preview** mode in Google Tag Manager
2. Navigate to any page on your site with `?zcId=test123` appended to the URL
3. Verify the ZeroClick global click tracking tag fires
4. Check that no errors appear in the tag debug output

### Browser Developer Tools

1. Open your browser's Developer Tools (F12)
2. Navigate to a page on your site with `?zcId=test123` in the URL
3. Check **Application** > **Cookies** for a `_zcId` cookie with value `test123`
4. Check **Application** > **Local Storage** for a `_zcId` entry with value `test123`

### Full Flow Test

1. Visit your site with `?zcId=test123` in the URL
2. Verify `_zcId` cookie and localStorage entry are set
3. Navigate to your conversion/thank-you page
4. Verify the ZeroClick Conversion Tag reads the stored `_zcId` and includes it in the conversion request

## Browser Compatibility

This tag is compatible with all modern browsers that support:
- Cookies
- localStorage
- URL query parameter parsing

Because this tag uses **first-party cookies** (set on your domain), it works in all browsers regardless of third-party cookie restrictions (Safari ITP, Firefox ETP, Chrome Privacy Sandbox).

## Privacy & Data Collection

The ZeroClick Global Click Tracking tag:
- Only activates when a `zcId` query parameter is present in the URL
- Stores only the ZeroClick visitor identifier -- no PII is collected
- Uses first-party cookies on your domain (not third-party)
- Does not make any network requests -- it only stores data locally

## Troubleshooting

### Cookie Not Being Set

- Verify `?zcId=xxx` is present in the URL when the page loads
- Check that the tag trigger is set to **All Pages**
- Ensure GTM container is published (not just in preview)

### Conversion Tag Can't Find zcId

- Verify this global tag is installed and firing on all pages
- Check that the user's first visit included `?zcId=xxx` in the URL
- Verify the cookie hasn't expired (30-day expiry)
- Check if the user cleared cookies between the click and conversion

### Tag Fires But No Cookie

- Check browser cookie settings -- some privacy extensions block all cookies
- Verify the site is served over HTTPS (the cookie uses `secure: true`)

## Additional Resources

- **ZeroClick Homepage**: [https://zeroclick.ai](https://zeroclick.ai)
- **Documentation**: [https://zeroclick.ai/documentation-global-tag](https://zeroclick.ai/documentation-global-tag)
- **Conversion Tag**: [https://github.com/piedotorg/zeroclick-conversion-gtm-tpl](https://github.com/piedotorg/zeroclick-conversion-gtm-tpl)

## Repository Files

- `template.tpl` - Google Tag Manager custom template
- `metadata.yaml` - Template metadata and versioning
- `LICENSE` - Apache License 2.0

## License

Apache License 2.0 - See [LICENSE](LICENSE) file for details

## Support

For support or questions about ZeroClick tracking:
- Visit [https://zeroclick.ai](https://zeroclick.ai)
- Check the official documentation at [https://zeroclick.ai/documentation-global-tag](https://zeroclick.ai/documentation-global-tag)

## Version History

See [metadata.yaml](metadata.yaml) for version history and change notes.

---

**Made for ZeroClick Advertising Platform** | [zeroclick.ai](https://zeroclick.ai)
