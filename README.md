# Israel VFR Plotter — Android (tablet) app

This wraps the Israel VFR Plotter web app in a full-screen Android WebView, set up for
tablets (e.g. Lenovo Tab / IdeaPad-class devices). It bundles the whole app offline as an
asset; only the live data (map tiles, OpenAIP layer, Open-Meteo weather) needs internet.

The app's screen (toolbar, plotter, drawing, routes, lat/lon grid, GPS, weather) is exactly
the web version — this project just makes it an installable `.apk`.

## What you need (one-time)
- **Android Studio** (free): https://developer.android.com/studio
  It includes the Java 17 JDK and the Android SDK, which are required to build an APK.
  (An APK cannot be produced without the Android SDK — that's why this is a project, not a prebuilt APK.)

## Build the APK — Android Studio (easiest)
1. Launch Android Studio → **Open** → select this `IsraelVFRPlotter` folder.
2. Wait for **Gradle sync** to finish (it downloads Gradle 8.2 + SDK bits the first time).
   If it offers to install a missing SDK / build-tools, accept.
3. Menu **Build → Build App Bundle(s) / APK(s) → Build APK(s)**.
4. When it finishes, click **locate** in the popup, or find it at:
   `app/build/outputs/apk/debug/app-debug.apk`

## Build the APK — command line (alternative)
From this folder:
```
./gradlew assembleDebug         # macOS/Linux
gradlew.bat assembleDebug       # Windows
```
APK appears at `app/build/outputs/apk/debug/app-debug.apk`.

## Release-signed APK
The project is wired for release signing. Without a key it still builds a release APK signed
with the debug key; to sign with your own key:

1. Create a keystore once (needs the JDK that ships with Android Studio on your PATH, or use Studio's
   **Build → Generate Signed Bundle / APK** wizard which does all of this for you):
   ```
   keytool -genkeypair -v -keystore release.keystore -alias vfr -keyalg RSA -keysize 2048 -validity 10000
   ```
2. Copy `keystore.properties.example` to `keystore.properties` and fill in your store/key passwords and alias.
   (`keystore.properties` and `*.keystore` are git-ignored so your secrets stay local.)
3. Build:
   ```
   ./gradlew assembleRelease
   ```
   Output: `app/build/outputs/apk/release/app-release.apk` — signed and zipaligned, ready to install or publish.

Or just use Android Studio's **Build → Generate Signed Bundle / APK → APK** wizard and follow the prompts.

## Icon & splash
The app ships with a custom adaptive launcher icon (an amber compass rose on the dark cockpit
background) and a matching splash screen shown on launch. To change them, edit
`res/drawable/ic_launcher_foreground.xml` (the artwork), `res/values/colors.xml` (`ic_bg`),
and `res/drawable/splash.xml`.

(Minimum Android version is 8.0 / API 26 — fine for any current Lenovo tablet.)

## Install on the tablet
1. Copy `app-debug.apk` to the tablet (USB, Drive, email, etc.).
2. On the tablet, open the file. Android will ask to allow **install from unknown sources** —
   allow it for your file manager / browser, then install.
3. Launch **Israel VFR Plotter**. On first run it asks for **Location** permission (for the GPS dot).

## Notes
- Needs internet for the map tiles, the **Open VFR (OpenAIP)** layer, and **WX** (Open-Meteo) weather.
- Paste your free **OpenAIP key** in-app (Open VFR / Aero button) the first time; it's remembered.
- Your **own licensed chart** loads via the **Chart** button (the native file picker is wired up).
- The app remembers settings, toolbar layout, and your OpenAIP key on the device.
- Orientation is free (portrait or landscape); the plotter has more room in landscape.

## Updating the app later
The whole app is the single file `app/src/main/assets/index.html`.
Replace that file with a newer version and rebuild — nothing else changes.

## No-build option
If you just want it running today without building: open `app/src/main/assets/index.html`
in **Chrome** on the tablet and use **⋮ → Add to Home screen** for a full-screen, app-like shortcut.
