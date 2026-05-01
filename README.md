# QR Scanner — Application Flutter Premium

## 📱 Description

Application mobile Flutter de scan QR Code avec UI premium (glassmorphism, laser animé, dark theme).

## 🚀 Démarrage rapide

```bash
# 1. Installer les dépendances
flutter pub get

# 2. Lancer l'application
flutter run

# 3. Build de production
flutter build apk --release        # Android
flutter build ios --release        # iOS
```

## 📦 Packages utilisés

| Package | Version | Usage |
|---------|---------|-------|
| `mobile_scanner` | ^5.2.3 | Scan QR (ML Kit / Vision) |
| `url_launcher` | ^6.3.1 | Ouverture URLs |
| `permission_handler` | ^11.3.1 | Permission caméra |
| `fluttertoast` | ^8.2.8 | Messages toast |
| `shared_preferences` | ^2.3.2 | Historique persistant |
| `uuid` | ^4.4.2 | IDs uniques historique |

## 🗂️ Structure du projet

```
lib/
├── main.dart                          # Point d'entrée
├── theme/
│   └── app_theme.dart                 # Thème + tokens design
├── models/
│   └── scan_result_model.dart         # Modèle historique
├── services/
│   ├── history_service.dart           # Persistance historique
│   └── url_service.dart               # Validation + ouverture URLs
├── widgets/
│   ├── scanner_overlay.dart           # Cadre + laser animé
│   ├── glass_button.dart              # Glassmorphism containers/buttons
│   ├── permission_request_view.dart   # Vue permission caméra
│   └── success_flash.dart             # Animation succès
└── screens/
    ├── scanner_screen.dart            # Écran principal
    └── history_screen.dart            # Écran historique
```

## 🎨 Décisions techniques

### Animation laser (scanner_overlay.dart)
- **CustomPainter** pour le cadre statique → zéro rebuild Flutter
- **AnimatedBuilder + Tween** pour la ligne laser → ne reconstruit QUE la ligne
- Glow via `MaskFilter.blur` + gradient transparent→cyan→transparent

### Glassmorphism (glass_button.dart)
```dart
ClipRRect → BackdropFilter(blur: 14) → Container(color: white.withOpacity(0.12))
```
C'est la seule façon de flouter ce qui est DERRIÈRE en Flutter.

### Debounce du scan
Variable `_isProcessing` qui bloque les détections multiples pendant le traitement.

## 📱 Permissions requises

### Android (AndroidManifest.xml)
- `CAMERA`
- `INTERNET`
- `READ_MEDIA_IMAGES` (Android 13+)
- `READ_EXTERNAL_STORAGE` (Android < 13)
- `VIBRATE`

### iOS (Info.plist)
- `NSCameraUsageDescription`
- `NSPhotoLibraryUsageDescription`
