# Changelog

All notable changes to the Silktide Cookie Consent Manager will be documented in this file.

## [2.0.0] - 2026-01-02

### 🎉 Major Release - Shadow DOM Integration

This is a major breaking change release that implements Shadow DOM for complete CSS isolation.

### Added
- **Shadow DOM Support**: Widget now uses Shadow DOM for complete CSS encapsulation
- **Single Script Import**: CSS is now embedded in JavaScript - no separate CSS file needed
- **Automatic Fallback**: Graceful degradation for browsers without Shadow DOM support
- **CSS Variables**: Centralized theming with CSS custom properties
- **Comprehensive Documentation**: Complete README with examples and API reference
- **Example Files**: 
  - `example-basic.html` - Simple implementation example
  - `example-google-consent-mode.html` - GTM integration example
- **Better Debugging**: Console logs for Shadow DOM status and CSS isolation verification

### Changed
- **BREAKING**: CSS must now be removed from HTML `<link>` tags
- **BREAKING**: Widget wrapper styles now applied via JavaScript instead of CSS
- **Improved**: CSS isolation prevents site styles from affecting widget
- **Improved**: Widget styles cannot leak into site CSS
- **Updated**: Universal CSS reset strategy for inherited properties
- **Updated**: CSS variables moved to Shadow DOM root level

### Fixed
- CSS inheritance issues where site styles affected widget appearance
- Modal positioning in Shadow DOM (now properly centered)
- Font and color inheritance from host document
- Button styling affected by global site CSS
- Text elements affected by site typography rules

### Migration Guide

**Before (v1.x):**
```html
<link rel="stylesheet" href="silktide-consent-manager.css">
<script src="silktide-consent-manager.js"></script>
```

**After (v2.0):**
```html
<script src="silktide-consent-manager.js"></script>
```

That's it! The CSS is now embedded in the JavaScript file. Your configuration code remains the same.

### Technical Details

#### Shadow DOM Implementation
- Uses `attachShadow({ mode: 'open' })` for maximum compatibility
- CSS injected as `<style>` tag inside Shadow Root
- Wrapper element styles applied via JavaScript `style.cssText`
- CSS variables defined at `:root` and `:host` level
- Universal selector (`*`) resets all inherited properties

#### Browser Support
- **Modern Browsers**: Full Shadow DOM support
  - Chrome 53+
  - Firefox 63+
  - Safari 10.1+
  - Edge 79+
- **Legacy Browsers**: Automatic fallback to scoped CSS
  - Injects CSS into document `<head>`
  - Uses ID-prefixed selectors
  - Warns in console about degraded isolation

#### Why Shadow DOM?
The previous implementation used ID-prefixed selectors (`#silktide-wrapper`) for scoping, but this approach had limitations:

1. **CSS Inheritance**: Inherited properties (color, font-family, etc.) passed from site to widget
2. **Specificity Wars**: Site CSS with `!important` could override widget styles
3. **Bidirectional Leakage**: Widget CSS could potentially affect site elements

Shadow DOM solves these issues by creating a true style boundary:
- ✅ Blocks direct selectors from light DOM
- ✅ Creates new style context
- ✅ Still allows CSS variables to pass through (for theming)
- ✅ Prevents widget styles from leaking out

## [1.0.0] - Previous Version

### Features
- Cookie consent banner
- Preferences modal
- Cookie icon
- Google Consent Mode support
- Customizable text and styling
- localStorage-based persistence
- Callback system
- Accessibility features

