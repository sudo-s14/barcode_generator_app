# BarCode Generator App - Project Plan

## 1. Project Overview

**App Name:** BarCode Generator
**Platforms:** iOS & Android
**Framework:** Flutter (Dart)
**Description:** A cross-platform mobile app that generates various barcode formats from user input, with options to save, share, customize, and manage barcodes. Built with platform-adaptive UI following Apple Human Interface Guidelines (HIG) and Google Material 3 Design Guidelines.

---

## 2. Tech Stack

| Layer              | Technology                          |
|--------------------|-------------------------------------|
| Framework          | Flutter 3.x                         |
| Language           | Dart 3.x                            |
| State Management   | flutter_bloc (Bloc/Cubit)           |
| Navigation         | GoRouter                            |
| Barcode Generation | `barcode` (pure Dart)               |
| Barcode Rendering  | `barcode_widget` (SVG)              |
| Local Storage      | Hive (NoSQL, fast)                  |
| Image Export       | `screenshot` + `image_gallery_saver`|
| Sharing            | `share_plus`                        |
| File System        | `path_provider`                     |
| Color Picker       | `flex_color_picker`                 |
| App Icon           | `flutter_launcher_icons`            |
| Splash Screen      | `flutter_native_splash`             |
| Theming            | Material 3 (Android) / Cupertino (iOS) |

---

## 3. Supported Barcode Formats

### 1D Barcodes
- Code 128 (A, B, C)
- Code 39
- Code 93
- EAN-13
- EAN-8
- UPC-A
- UPC-E
- ITF / ITF-14
- Codabar

### 2D Barcodes (Stretch Goal)
- QR Code
- Data Matrix
- PDF417
- Aztec

---

## 4. Core Features

### 4.1 Barcode Generation
- Select barcode format from categorized dropdown
- Text/number input field with format-specific validation
- Instant barcode rendering on input change
- Error feedback for invalid input

### 4.2 Live Preview
- Real-time SVG barcode rendering as user types
- Pinch-to-zoom on generated barcode
- Landscape mode support for full-width preview

### 4.3 Customization
- Foreground color (bar color)
- Background color
- Bar width adjustment
- Barcode height adjustment
- Show/hide human-readable text label
- Text font size control

### 4.4 Save to Gallery
- Export barcode as PNG image
- Configurable export resolution (1x, 2x, 3x)
- Save confirmation with thumbnail preview

### 4.5 Share
- Native share sheet integration (AirDrop, Messages, Email, etc.)
- Share as image file
- Copy barcode image to clipboard

### 4.6 History
- Auto-save every generated barcode
- List view with barcode thumbnail, format, value, timestamp
- Search and filter by format or value
- Swipe-to-delete (iOS) / long-press delete (Android)
- Clear all history option

### 4.7 Favorites
- Pin/unpin barcodes from history
- Dedicated favorites tab for quick access
- Reorder favorites via drag-and-drop

### 4.8 Batch Generation
- Multi-line text input (one value per line)
- Select single format for all entries
- Preview all generated barcodes in scrollable list
- Export all as individual images or single PDF

---

## 5. Screens & Navigation

### Navigation Structure
- **Bottom Navigation** with 4 tabs: Generate, History, Favorites, Settings
- **Stack Navigation** for sub-screens (Customize, Batch, Barcode Detail)

### Screen Breakdown

| Screen             | Route           | Description                                      |
|--------------------|-----------------|--------------------------------------------------|
| Home / Generate    | `/`             | Format picker + input + live preview + action buttons |
| Customize          | `/customize`    | Color, size, label options for current barcode   |
| Barcode Detail     | `/detail/:id`   | Full-screen view of a saved barcode              |
| History            | `/history`      | Chronological list of generated barcodes         |
| Favorites          | `/favorites`    | Pinned barcodes grid/list                        |
| Batch Generate     | `/batch`        | Multi-value input + bulk preview                 |
| Settings           | `/settings`     | Theme, defaults, about, export preferences       |

---

## 6. Platform-Adaptive UI Design

### 6.1 iOS — Apple Human Interface Guidelines

| Element            | Implementation                                    |
|--------------------|---------------------------------------------------|
| Navigation Bar     | `CupertinoNavigationBar` with large titles        |
| Tab Bar            | Bottom tab bar with SF Symbols-style icons        |
| Typography         | San Francisco font via `CupertinoThemeData`       |
| Modals             | iOS-style bottom sheets and action sheets         |
| Lists              | `CupertinoListSection` with inset grouped style   |
| Alerts             | `CupertinoAlertDialog`                            |
| Switches           | `CupertinoSwitch`                                 |
| Scroll Physics     | `BouncingScrollPhysics` (rubber-band effect)      |
| Haptics            | Taptic feedback on generate/save/delete actions   |
| Gestures           | Swipe-back navigation, swipe-to-delete            |
| Safe Areas         | Respect notch, Dynamic Island, home indicator     |
| Dark Mode          | System-aware via `MediaQuery.platformBrightnessOf` |

### 6.2 Android — Material 3 / Material You

| Element            | Implementation                                    |
|--------------------|---------------------------------------------------|
| App Bar            | Material 3 `SliverAppBar` with collapsing toolbar |
| Navigation Bar     | Material 3 `NavigationBar` with dynamic color     |
| Typography         | Roboto via Material 3 type scale                  |
| Modals             | Material bottom sheets and dialogs                |
| Lists              | `ListTile` with Material 3 styling                |
| Alerts             | `AlertDialog` with Material 3 buttons             |
| Switches           | Material 3 `Switch`                               |
| Scroll Physics     | `ClampingScrollPhysics`                           |
| FAB                | `FloatingActionButton` for primary generate action|
| Dynamic Color      | `dynamic_color` package for Material You theming  |
| Gestures           | Predictive back gesture (Android 14+)             |
| Edge-to-edge       | Transparent system bars, proper insets            |
| Dark Mode          | System-aware via `ThemeMode.system`               |

### 6.3 Adaptive Widget Strategy

```
Platform.isIOS  → Cupertino widgets, iOS navigation patterns
Platform.isAndroid → Material 3 widgets, Android navigation patterns
```

Key adaptive wrappers to build:
- `AdaptiveScaffold` — CupertinoPageScaffold vs Material Scaffold
- `AdaptiveDialog` — CupertinoAlertDialog vs AlertDialog
- `AdaptiveBottomSheet` — CupertinoActionSheet vs BottomSheet
- `AdaptiveSwitch` — CupertinoSwitch vs Switch
- `AdaptiveTabBar` — CupertinoTabBar vs NavigationBar

---

## 7. App Icon

### Design Concept
- Minimal barcode glyph on solid/gradient background
- No text in icon (both platforms discourage it)
- Recognizable at all sizes (16x16 to 1024x1024)

### iOS Requirements
| Spec               | Detail                                          |
|---------------------|------------------------------------------------|
| Source Format        | 1024x1024 PNG, no transparency, opaque bg      |
| Shape               | Square asset, iOS applies rounded-rect mask     |
| Sizes Auto-generated| 60x60, 120x120, 180x180, etc.                 |
| No alpha channel    | Required by App Store                           |

### Android Requirements
| Spec               | Detail                                          |
|---------------------|------------------------------------------------|
| Adaptive Icon       | Foreground (108x108dp) + Background layer      |
| Safe Zone           | Content within 72dp center circle              |
| Format              | Vector drawable (XML) or PNG                   |
| Legacy Fallback     | 48dp icon for pre-Android 8 devices            |
| Play Store          | 512x512 high-res icon                          |

### Configuration
```yaml
# pubspec.yaml
dev_dependencies:
  flutter_launcher_icons: ^0.14.x

flutter_launcher_icons:
  android: true
  ios: true
  image_path: "assets/icon/app_icon.png"
  adaptive_icon_background: "#1A1A2E"
  adaptive_icon_foreground: "assets/icon/app_icon_foreground.png"
  min_sdk_android: 21
```

### Asset Files
```
assets/icon/
├── app_icon.png                  # 1024x1024 source (iOS + fallback)
└── app_icon_foreground.png       # Android adaptive foreground layer
```

---

## 8. Splash Screen

### iOS Requirements (Launch Screen)
| Spec               | Detail                                          |
|---------------------|------------------------------------------------|
| Format              | Storyboard-based (required by Apple)           |
| Content             | App icon/logo centered + solid background      |
| No text/taglines    | Apple discourages promotional content          |
| Transition          | Must match initial app screen for smooth launch|

### Android Requirements (Splash Screen API)
| Spec               | Detail                                          |
|---------------------|------------------------------------------------|
| API                 | Android 12+ SplashScreen API (automatic)       |
| Icon                | Adaptive icon centered, max 240dp              |
| Background          | Single solid color                             |
| Animated (optional) | AVD animation, max 1000ms                      |
| Branding            | Not recommended by Google                      |

### Configuration
```yaml
# pubspec.yaml
dev_dependencies:
  flutter_native_splash: ^2.4.x

flutter_native_splash:
  color: "#1A1A2E"
  image: "assets/splash/splash_logo.png"
  android_12:
    icon_background_color: "#1A1A2E"
    image: "assets/splash/splash_icon.png"
  ios: true
  android: true
```

### Asset Files
```
assets/splash/
├── splash_logo.png               # Centered logo for splash screen
└── splash_icon.png               # Android 12 adaptive splash icon
```

---

## 9. Project Structure

```
barcode_generator/
├── android/                        # Android native config
├── ios/                            # iOS native config
├── assets/
│   ├── icon/
│   │   ├── app_icon.png
│   │   └── app_icon_foreground.png
│   ├── splash/
│   │   ├── splash_logo.png
│   │   └── splash_icon.png
│   └── fonts/                      # Custom fonts (if any)
├── lib/
│   ├── main.dart                   # Entry point
│   ├── app.dart                    # MaterialApp/CupertinoApp, theme, router
│   ├── router/
│   │   └── app_router.dart         # GoRouter configuration
│   ├── core/
│   │   ├── barcode_engine.dart     # Barcode generation logic wrapper
│   │   ├── validators.dart         # Input validation per format
│   │   ├── export_service.dart     # Save to gallery, share, clipboard
│   │   ├── constants.dart          # Format configs, color defaults
│   │   └── extensions.dart         # Dart extension methods
│   ├── shared/
│   │   ├── widgets/
│   │   │   ├── adaptive_scaffold.dart
│   │   │   ├── adaptive_dialog.dart
│   │   │   ├── adaptive_bottom_sheet.dart
│   │   │   ├── adaptive_switch.dart
│   │   │   └── barcode_card.dart
│   │   ├── theme/
│   │   │   ├── app_theme.dart      # Material 3 + Cupertino themes
│   │   │   ├── app_colors.dart     # Color palette
│   │   │   └── app_typography.dart # Text styles
│   │   └── models/
│   │       └── barcode_entry.dart  # Data model for saved barcodes
│   ├── features/
│   │   ├── generator/
│   │   │   ├── screens/
│   │   │   │   ├── home_screen.dart
│   │   │   │   └── customize_screen.dart
│   │   │   ├── widgets/
│   │   │   │   ├── barcode_preview.dart
│   │   │   │   ├── format_picker.dart
│   │   │   │   └── input_field.dart
│   │   │   └── bloc/
│   │   │       ├── generator_bloc.dart
│   │   │       ├── generator_event.dart
│   │   │       └── generator_state.dart
│   │   ├── history/
│   │   │   ├── screens/
│   │   │   │   └── history_screen.dart
│   │   │   ├── widgets/
│   │   │   │   └── history_list.dart
│   │   │   └── bloc/
│   │   │       ├── history_bloc.dart
│   │   │       ├── history_event.dart
│   │   │       └── history_state.dart
│   │   ├── favorites/
│   │   │   ├── screens/
│   │   │   │   └── favorites_screen.dart
│   │   │   └── bloc/
│   │   │       ├── favorites_bloc.dart
│   │   │       ├── favorites_event.dart
│   │   │       └── favorites_state.dart
│   │   ├── batch/
│   │   │   ├── screens/
│   │   │   │   └── batch_screen.dart
│   │   │   └── bloc/
│   │   │       ├── batch_bloc.dart
│   │   │       ├── batch_event.dart
│   │   │       └── batch_state.dart
│   │   └── settings/
│   │       ├── screens/
│   │       │   └── settings_screen.dart
│   │       └── cubit/
│   │           └── settings_cubit.dart
├── test/
│   ├── unit/
│   │   ├── barcode_engine_test.dart
│   │   └── validators_test.dart
│   ├── widget/
│   │   ├── home_screen_test.dart
│   │   └── barcode_preview_test.dart
│   └── integration/
│       └── app_test.dart
├── integration_test/
│   └── app_integration_test.dart
├── pubspec.yaml
├── analysis_options.yaml
└── PROJECT_PLAN.md
```

---

## 10. Dependencies (pubspec.yaml)

```yaml
name: barcode_generator
description: A barcode generator app for iOS and Android
version: 1.0.0+1

environment:
  sdk: ">=3.0.0 <4.0.0"

dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.8

  # Barcode
  barcode_widget: ^2.6.3

  # State Management
  flutter_bloc: ^8.1.6
  equatable: ^2.0.5

  # Navigation
  go_router: ^14.2.7

  # Local Storage
  hive_flutter: ^1.1.0

  # Export & Share
  screenshot: ^3.0.0
  image_gallery_saver: ^2.0.3
  share_plus: ^9.0.0
  path_provider: ^2.1.4

  # UI
  flex_color_picker: ^3.5.1
  dynamic_color: ^1.7.0

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^4.0.0
  build_runner: ^2.4.12
  hive_generator: ^2.0.1
  bloc_test: ^9.1.7
  flutter_launcher_icons: ^0.14.1
  flutter_native_splash: ^2.4.1

flutter:
  uses-material-design: true
  assets:
    - assets/icon/
    - assets/splash/
```

---

## 11. Data Models

### BarcodeEntry (Hive + Equatable Model)
```dart
@HiveType(typeId: 0)
class BarcodeEntry extends Equatable {
  @HiveField(0)
  final String id;              // UUID

  @HiveField(1)
  final String value;           // Input text/number

  @HiveField(2)
  final String format;          // Barcode format name

  @HiveField(3)
  final DateTime createdAt;     // Timestamp

  @HiveField(4)
  final bool isFavorite;        // Pinned status

  @HiveField(5)
  final int foregroundColor;    // Bar color (as int)

  @HiveField(6)
  final int backgroundColor;   // Background color (as int)

  @HiveField(7)
  final double barWidth;        // Bar width

  @HiveField(8)
  final double height;          // Barcode height

  @HiveField(9)
  final bool showText;          // Show human-readable text

  const BarcodeEntry({...});

  BarcodeEntry copyWith({...});  // Immutable updates for Bloc state

  @override
  List<Object?> get props => [id, value, format, createdAt,
    isFavorite, foregroundColor, backgroundColor, barWidth, height, showText];
}
```

---

## 12. Development Phases & Timeline

### Phase 1 — Project Setup & Core Generation (Week 1–2)

| Task | Details |
|------|---------|
| Project init | `flutter create --org com.yourname barcode_generator` |
| Dependencies | Add all packages to pubspec.yaml |
| Folder structure | Create feature-based directory layout |
| Routing | Configure GoRouter with all routes |
| Theming | Set up Material 3 + Cupertino adaptive themes |
| Home screen | Format dropdown, text input, generate button |
| Barcode preview | Integrate `barcode_widget` for live SVG rendering |
| Validation | Format-specific input validators |
| Adaptive scaffold | Build platform-aware base layout |

**Deliverable:** App generates and displays barcodes in all 1D formats.

---

### Phase 2 — Customization & Persistence (Week 3)

| Task | Details |
|------|---------|
| Customize screen | Color pickers (foreground/background) |
| Size controls | Sliders for bar width, height |
| Label toggle | Show/hide text under barcode |
| Hive setup | Initialize Hive, register adapters |
| BarcodeEntry model | Create model with Hive annotations |
| Auto-save | Save barcode to history on generation |
| History screen | List view with thumbnails, search, filter |
| Delete | Swipe-to-delete (iOS) / long-press (Android) |
| Favorites | Toggle favorite, dedicated favorites screen |

**Deliverable:** Full customization + persistent history and favorites.

---

### Phase 3 — Export, Share & Batch (Week 4)

| Task | Details |
|------|---------|
| Screenshot capture | Render barcode widget to PNG bytes |
| Save to gallery | Write PNG to device photo library |
| Share | Native share sheet with image attachment |
| Clipboard | Copy barcode image to clipboard |
| Batch screen | Multi-line input, single format selection |
| Batch preview | Scrollable list of generated barcodes |
| Batch export | Save all as individual images |

**Deliverable:** Complete export/share pipeline + batch generation.

---

### Phase 4 — Polish, Animations & Accessibility (Week 5)

| Task | Details |
|------|---------|
| Animations | Hero transitions, fade-ins, slide transitions |
| Empty states | Illustrated empty states for history/favorites |
| Loading states | Shimmer/skeleton loading indicators |
| Error handling | Snackbars/toasts for errors and confirmations |
| Haptic feedback | Vibration on generate, save, delete (iOS + Android) |
| Accessibility | Semantic labels, large text, screen reader support |
| Dark mode | System-aware + manual toggle in settings |
| Dynamic color | Material You color extraction on Android |
| App icon | Generate all platform sizes via flutter_launcher_icons |
| Splash screen | Configure via flutter_native_splash |

**Deliverable:** Polished, accessible, production-quality UI.

---

### Phase 5 — Testing & Release (Week 6)

| Task | Details |
|------|---------|
| Unit tests | barcode_engine, validators, blocs (via bloc_test) |
| Widget tests | All screens and key widgets |
| Integration tests | Full user flows (generate → save → share) |
| Performance | Profile on real iOS + Android devices |
| iOS build | Archive, TestFlight internal testing |
| Android build | App Bundle, internal testing track |
| App Store assets | Screenshots, description, keywords, categories |
| Play Store assets | Feature graphic, screenshots, listing |
| Submission | Submit to App Store + Play Store review |

**Deliverable:** App live on both stores.

---

## 13. Non-Functional Requirements

| Requirement      | Target                                    |
|------------------|-------------------------------------------|
| Render Speed     | Barcode generates in <100ms               |
| Offline          | Fully functional without network           |
| App Size         | <20MB installed                            |
| Min iOS          | iOS 15.0+                                 |
| Min Android      | API 26 (Android 8.0)+                     |
| Accessibility    | WCAG 2.1 AA / platform guidelines         |
| Frame Rate       | 60fps smooth scrolling and transitions     |
| Storage          | <50MB local data for 10,000 saved barcodes |

---

## 14. App Store & Play Store Metadata

### App Store (iOS)
- **Category:** Utilities
- **Subtitle:** Generate & Share Barcodes
- **Keywords:** barcode, generator, code128, ean13, upc, scanner, label
- **Age Rating:** 4+
- **Screenshots:** 6.7" (iPhone 15 Pro Max), 6.1" (iPhone 15), 12.9" (iPad Pro)

### Play Store (Android)
- **Category:** Tools
- **Short Description:** Generate, customize, and share barcodes instantly
- **Feature Graphic:** 1024x500 banner
- **Screenshots:** Phone (16:9), 7" tablet, 10" tablet
- **Content Rating:** Everyone

---

## 15. Future Enhancements (Post-MVP)

| Feature                  | Description                                    |
|--------------------------|------------------------------------------------|
| Barcode Scanner          | Camera-based scanning via `mobile_scanner`     |
| Cloud Sync               | Firebase/Supabase sync across devices          |
| PDF Export               | Export barcodes as formatted PDF with labels   |
| Home Screen Widgets      | iOS WidgetKit / Android Glance widgets         |
| Barcode Templates        | Pre-built layouts (shipping, inventory, retail)|
| Localization             | Multi-language support (i18n)                  |
| Apple Watch / Wear OS    | Quick barcode display on wearables             |
| Bulk Import              | Import values from CSV/Excel files             |
| Print Support            | Direct printing via AirPrint / Android Print   |

---

## 16. Risks & Mitigations

| Risk | Mitigation |
|------|-----------|
| Platform-specific bugs | Test on real iOS + Android devices, not just simulators |
| Barcode rendering accuracy | Validate against physical barcode scanners |
| App Store rejection | Follow all HIG/Material guidelines strictly |
| Large image export OOM | Limit max export resolution, use isolates for processing |
| Hive migration issues | Version data models, write migration logic early |

---

## 17. Asset Creation Note

The following assets require graphic design work (outside of code generation):

1. **App Icon** — 1024x1024 PNG source file
2. **App Icon Foreground** — 108dp transparent PNG for Android adaptive icons
3. **Splash Logo** — Centered logo for splash screen
4. **Empty State Illustrations** — For history/favorites empty states
5. **App Store Screenshots** — Framed device screenshots with captions
6. **Play Store Feature Graphic** — 1024x500 promotional banner

**Recommended tools:** Figma, Canva, IconKitchen (icons), or hire a designer.

---

*Last updated: 2026-04-05*
*Framework: Flutter | Platforms: iOS & Android*
