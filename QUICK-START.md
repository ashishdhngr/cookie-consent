# Quick Start Guide

Get up and running with Silktide Cookie Consent Manager in 5 minutes.

## Step 1: Include the Script

Add one line to your HTML:

```html
<script src="silktide-consent-manager.js"></script>
```

That's it! No CSS file needed.

## Step 2: Configure

Add your configuration right after the script:

```html
<script>
  silktideCookieBannerManager.updateCookieBannerConfig({
    cookieTypes: [
      {
        id: "necessary",
        name: "Necessary",
        description: "<p>Required for the site to work.</p>",
        required: true
      },
      {
        id: "analytics",
        name: "Analytics",
        description: "<p>Helps us improve the site.</p>",
        defaultValue: true,
        onAccept: function() {
          // Load your analytics here
          console.log('Analytics enabled');
        },
        onReject: function() {
          console.log('Analytics disabled');
        }
      }
    ]
  });
</script>
```

## Step 3: Test

1. Open your page
2. See the cookie banner
3. Make a choice
4. Check the console logs

## Next Steps

### Add More Cookie Types

```javascript
{
  id: "advertising",
  name: "Advertising",
  description: "<p>For personalized ads.</p>",
  onAccept: function() {
    // Load ad scripts
  }
}
```

### Integrate with Google Tag Manager

```javascript
onAccept: function() {
  gtag('consent', 'update', {
    analytics_storage: 'granted'
  });
}
```

See `example-google-consent-mode.html` for complete GTM integration.

### Customize Text

```javascript
text: {
  banner: {
    description: "<p>Your custom message here</p>",
    acceptAllButtonText: "OK",
    rejectNonEssentialButtonText: "No Thanks"
  }
}
```

### Change Position

```javascript
position: {
  banner: "center" // or "bottomLeft", "bottomCenter", "bottomRight"
}
```

## Common Tasks

### Reset Consent (for testing)
```javascript
localStorage.clear();
location.reload();
```

### Check if consent was given
```javascript
const choice = localStorage.getItem('silktideCookieChoice_analytics');
if (choice === 'true') {
  // Analytics accepted
}
```

### Manually open preferences
The cookie icon appears after initial choice. Click it to reopen preferences.

## Files in This Package

- `silktide-consent-manager.js` - Main widget file (required)
- `silktide-consent-manager.css` - Legacy CSS file (not needed for v2.0+)
- `README.md` - Complete documentation
- `QUICK-START.md` - This file
- `CHANGELOG.md` - Version history
- `example-basic.html` - Simple example
- `example-google-consent-mode.html` - GTM integration example
- `test.html` - CSS isolation test

## Need Help?

- 📖 Read `README.md` for complete documentation
- 💡 Check examples in `example-*.html` files
- 🐛 Test CSS isolation with `test.html`
- 📝 See `CHANGELOG.md` for recent changes

## Browser Support

- ✅ Chrome 53+
- ✅ Firefox 63+
- ✅ Safari 10.1+
- ✅ Edge 79+
- ⚠️ Older browsers: Fallback mode (reduced CSS isolation)

---

**That's it!** You're ready to go. 🎉

