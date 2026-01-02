# Silktide Cookie Consent Manager

A lightweight, customizable cookie consent widget with Shadow DOM isolation for complete CSS encapsulation. Works seamlessly with Google Consent Mode v2.

## Features

- ✅ **Complete CSS Isolation** - Shadow DOM prevents style leakage in both directions
- ✅ **Single Script Import** - No separate CSS file needed
- ✅ **Google Consent Mode v2 Compatible** - Built-in support for GTM integration
- ✅ **Fully Customizable** - Configure text, colors, and behavior
- ✅ **Accessible** - WCAG compliant with keyboard navigation and ARIA labels
- ✅ **Lightweight** - No dependencies, vanilla JavaScript
- ✅ **Browser Fallback** - Graceful degradation for browsers without Shadow DOM support

## Installation

### CDN (Recommended)

```html
<script src="https://your-cdn.com/silktide-consent-manager.js"></script>
```

### Self-Hosted

Download `silktide-consent-manager.js` and include it in your HTML:

```html
<script src="/path/to/silktide-consent-manager.js"></script>
```

## Basic Usage

### Minimal Setup

```html
<script src="silktide-consent-manager.js"></script>
<script>
  silktideCookieBannerManager.updateCookieBannerConfig({
    cookieTypes: [
      {
        id: "necessary",
        name: "Necessary",
        description: "<p>Essential cookies for site functionality.</p>",
        required: true
      },
      {
        id: "analytics",
        name: "Analytics",
        description: "<p>Help us improve the site.</p>",
        defaultValue: true
      }
    ]
  });
</script>
```

## Configuration

### Complete Configuration Example

```javascript
silktideCookieBannerManager.updateCookieBannerConfig({
  // Widget positioning
  position: {
    banner: "bottomRight" // Options: "bottomRight", "bottomLeft", "bottomCenter", "center"
  },
  
  // Cookie icon settings
  cookieIcon: {
    position: "bottomLeft", // Options: "bottomLeft", "bottomRight"
    colorScheme: "light" // Optional: custom color scheme class
  },
  
  // Backdrop settings
  background: {
    showBackground: true // Show backdrop overlay behind modal
  },
  
  // Cookie types and categories
  cookieTypes: [
    {
      id: "necessary",
      name: "Necessary",
      description: "<p>These cookies are required for the website to function.</p>",
      required: true,
      onAccept: function() {
        console.log('Necessary cookies loaded');
      }
    },
    {
      id: "analytics",
      name: "Analytics",
      description: "<p>Help us understand how visitors use our site.</p>",
      defaultValue: true,
      onAccept: function() {
        // Load analytics scripts
        gtag('consent', 'update', {
          analytics_storage: 'granted'
        });
      },
      onReject: function() {
        gtag('consent', 'update', {
          analytics_storage: 'denied'
        });
      }
    },
    {
      id: "advertising",
      name: "Advertising",
      description: "<p>Used to deliver personalized ads.</p>",
      defaultValue: false,
      onAccept: function() {
        gtag('consent', 'update', {
          ad_storage: 'granted',
          ad_user_data: 'granted',
          ad_personalization: 'granted'
        });
      },
      onReject: function() {
        gtag('consent', 'update', {
          ad_storage: 'denied',
          ad_user_data: 'denied',
          ad_personalization: 'denied'
        });
      }
    }
  ],
  
  // Customizable text
  text: {
    banner: {
      description: "<p>We use cookies to enhance your experience. <a href='/privacy'>Learn more</a></p>",
      acceptAllButtonText: "Accept all",
      acceptAllButtonAccessibleLabel: "Accept all cookies",
      rejectNonEssentialButtonText: "Reject non-essential",
      rejectNonEssentialButtonAccessibleLabel: "Reject non-essential cookies",
      preferencesButtonText: "Preferences",
      preferencesButtonAccessibleLabel: "Manage cookie preferences"
    },
    preferences: {
      title: "Customize your cookie preferences",
      description: "<p>Choose which cookies you want to allow.</p>",
      creditLinkText: "Get this banner for free",
      creditLinkAccessibleLabel: "Get this cookie banner for free from Silktide"
    }
  },
  
  // Event callbacks
  onBannerOpen: function() {
    console.log('Banner opened');
  },
  onBannerClose: function() {
    console.log('Banner closed');
  },
  onPreferencesOpen: function() {
    console.log('Preferences modal opened');
  },
  onPreferencesClose: function() {
    console.log('Preferences modal closed');
  },
  onBackdropOpen: function() {
    console.log('Backdrop shown');
  },
  onBackdropClose: function() {
    console.log('Backdrop hidden');
  },
  onAcceptAll: function() {
    console.log('All cookies accepted');
  },
  onRejectAll: function() {
    console.log('Non-essential cookies rejected');
  },
  
  // Optional: Banner suffix for multiple instances
  bannerSuffix: "mysite" // Creates unique localStorage keys
});
```

## Google Consent Mode Integration

### Setup Google Tag Manager

```html
<!-- Google Tag Manager (in <head>) -->
<script>
  // Initialize consent mode BEFORE GTM loads
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  
  // Set default consent state (denied until user accepts)
  gtag('consent', 'default', {
    'analytics_storage': 'denied',
    'ad_storage': 'denied',
    'ad_user_data': 'denied',
    'ad_personalization': 'denied',
    'wait_for_update': 500
  });
</script>

<!-- Google Tag Manager script -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXXX');</script>
```

### Configure Cookie Consent

```html
<script src="silktide-consent-manager.js"></script>
<script>
  silktideCookieBannerManager.updateCookieBannerConfig({
    cookieTypes: [
      {
        id: "necessary",
        name: "Necessary",
        required: true
      },
      {
        id: "analytics",
        name: "Analytics",
        onAccept: function() {
          gtag('consent', 'update', {
            analytics_storage: 'granted'
          });
          dataLayer.push({'event': 'consent_analytics_granted'});
        },
        onReject: function() {
          gtag('consent', 'update', {
            analytics_storage: 'denied'
          });
        }
      },
      {
        id: "advertising",
        name: "Advertising",
        onAccept: function() {
          gtag('consent', 'update', {
            ad_storage: 'granted',
            ad_user_data: 'granted',
            ad_personalization: 'granted'
          });
          dataLayer.push({'event': 'consent_advertising_granted'});
        },
        onReject: function() {
          gtag('consent', 'update', {
            ad_storage: 'denied',
            ad_user_data: 'denied',
            ad_personalization: 'denied'
          });
        }
      }
    ]
  });
</script>
```

## API Reference

### Methods

#### `updateCookieBannerConfig(config)`
Initialize or update the cookie banner configuration.

```javascript
silktideCookieBannerManager.updateCookieBannerConfig({
  cookieTypes: [...],
  text: {...}
});
```

#### `initCookieBanner()`
Manually initialize the cookie banner (usually called automatically).

```javascript
silktideCookieBannerManager.initCookieBanner();
```

#### `injectScript(url, loadOption)`
Helper method to inject external scripts based on consent.

```javascript
silktideCookieBannerManager.injectScript(
  'https://example.com/analytics.js',
  'async' // Options: 'async', 'defer', or null
);
```

### Configuration Options

#### `position`
- **Type:** `Object`
- **Properties:**
  - `banner`: `"bottomRight"` | `"bottomLeft"` | `"bottomCenter"` | `"center"`

#### `cookieIcon`
- **Type:** `Object`
- **Properties:**
  - `position`: `"bottomLeft"` | `"bottomRight"`
  - `colorScheme`: `String` (optional CSS class)

#### `background`
- **Type:** `Object`
- **Properties:**
  - `showBackground`: `Boolean` (show backdrop overlay)

#### `cookieTypes`
- **Type:** `Array<Object>`
- **Properties:**
  - `id`: `String` (unique identifier)
  - `name`: `String` (display name)
  - `description`: `String` (HTML description)
  - `required`: `Boolean` (cannot be disabled)
  - `defaultValue`: `Boolean` (default consent state)
  - `onAccept`: `Function` (called when accepted)
  - `onReject`: `Function` (called when rejected)

#### `text`
Customize all visible text in the widget.

#### `bannerSuffix`
- **Type:** `String`
- **Default:** `""`
- Create unique localStorage keys for multiple instances

### Events

All event callbacks receive no parameters and run in the main JavaScript context (not isolated by Shadow DOM).

- `onBannerOpen` - Banner is displayed
- `onBannerClose` - Banner is hidden
- `onPreferencesOpen` - Preferences modal opened
- `onPreferencesClose` - Preferences modal closed
- `onBackdropOpen` - Backdrop overlay shown
- `onBackdropClose` - Backdrop overlay hidden
- `onAcceptAll` - User accepts all cookies
- `onRejectAll` - User rejects non-essential cookies

## CSS Customization

The widget uses Shadow DOM for complete CSS isolation. To customize colors and styling, modify the CSS variables:

```javascript
// Currently, CSS variables are embedded in the JavaScript
// Future versions may support custom theming via configuration
```

### Default CSS Variables

```css
--focus: 0 0 0 2px #ffffff, 0 0 0 4px #000000, 0 0 0 6px #ffffff;
--fontFamily: Helvetica Neue, Segoe UI, Arial, sans-serif;
--primaryColor: #533BE2;
--backgroundColor: #FFFFFF;
--textColor: #4B494B;
--backdropBackgroundColor: #00000033;
--backdropBackgroundBlur: 0px;
--cookieIconColor: #4B494B;
--cookieIconBackgroundColor: #FFFFFF;
```

## localStorage Keys

The widget stores user preferences in localStorage:

- `silktideCookieBanner_InitialChoice{suffix}` - Tracks if user made a choice
- `silktideCookieChoice_{cookieId}{suffix}` - Individual cookie consent state

To reset consent (for testing):

```javascript
localStorage.clear();
// or remove specific keys:
localStorage.removeItem('silktideCookieBanner_InitialChoice');
```

## Browser Support

- **Modern Browsers:** Full Shadow DOM support (Chrome 53+, Firefox 63+, Safari 10.1+, Edge 79+)
- **Legacy Browsers:** Automatic fallback to standard DOM with scoped CSS

## Accessibility

- ✅ Keyboard navigation (Tab, Shift+Tab)
- ✅ Escape key to close modal
- ✅ Focus trap in banner and modal
- ✅ ARIA labels and roles
- ✅ Screen reader friendly

## Migration from v1

If upgrading from a version that required separate CSS:

**Before:**
```html
<link rel="stylesheet" href="silktide-consent-manager.css">
<script src="silktide-consent-manager.js"></script>
```

**After:**
```html
<script src="silktide-consent-manager.js"></script>
```

That's it! The CSS is now embedded in the JavaScript file.

## Troubleshooting

### CSS from my site is affecting the widget
- **Issue:** Site CSS is overriding widget styles
- **Solution:** Ensure you're using the Shadow DOM version. Check console for: `✅ Silktide Consent Manager: Shadow DOM enabled`
- **Fallback:** If Shadow DOM is not supported, the widget uses scoped CSS which may still be affected by very aggressive site styles

### Widget CSS is affecting my site
- **Issue:** Widget styles leak into the main site
- **Solution:** This should not happen with Shadow DOM. If it does, check that Shadow DOM is enabled and not in fallback mode

### Console errors about `gtag` not defined
- **Issue:** Google Tag Manager not loaded before consent manager
- **Solution:** Load GTM before the consent manager, or check for `typeof gtag === 'function'` before calling

### Consent not persisting
- **Issue:** User choices reset on page reload
- **Solution:** Check that localStorage is enabled and not blocked by browser privacy settings

## Examples

### Example 1: Simple setup with analytics only

```html
<script src="silktide-consent-manager.js"></script>
<script>
  silktideCookieBannerManager.updateCookieBannerConfig({
    cookieTypes: [
      {
        id: "necessary",
        name: "Necessary",
        description: "<p>Required for site functionality.</p>",
        required: true
      },
      {
        id: "analytics",
        name: "Analytics",
        description: "<p>Help us improve.</p>",
        defaultValue: true,
        onAccept: () => {
          // Load Google Analytics
          (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
          (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
          m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
          })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');
          ga('create', 'UA-XXXXX-Y', 'auto');
          ga('send', 'pageview');
        }
      }
    ]
  });
</script>
```

### Example 2: Full setup with Google Consent Mode v2

See the [Google Consent Mode Integration](#google-consent-mode-integration) section above.

### Example 3: Custom positioning and text

```javascript
silktideCookieBannerManager.updateCookieBannerConfig({
  position: {
    banner: "center"
  },
  cookieIcon: {
    position: "bottomRight"
  },
  text: {
    banner: {
      description: "<p>🍪 We use cookies! <a href='/privacy'>Privacy Policy</a></p>",
      acceptAllButtonText: "I Accept",
      rejectNonEssentialButtonText: "Only Necessary"
    }
  },
  cookieTypes: [/* ... */]
});
```

## License

This project is available under the MIT License.

## Support

For issues, questions, or contributions, visit:
- Website: https://silktide.com/consent-manager
- GitHub: [Repository URL]

## Credits

Developed by Silktide - Making the web a better place.
