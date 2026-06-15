# How to get the installable APK

The downloaded zip is the project source. An APK is produced by *building* it. Pick one:

## Option A — easiest, no APK, ~2 minutes
Put `app/src/main/assets/index.html` on the tablet, open it in **Chrome**,
then **⋮ → Add to Home screen**. Done — launches full-screen like an app.

## Option B — let GitHub build the APK for you (no software to install)
1. Create a free account at https://github.com and click **New repository**.
2. On the new repo page choose **uploading an existing file**, then drag in the
   contents of this project folder (keep the folders). Commit.
3. Open the **Actions** tab → the **Build APK** workflow runs automatically
   (or click **Run workflow**). Wait ~3–5 minutes for the green check.
4. Click the finished run → **Artifacts** → download **israel-vfr-plotter-apk**.
   Unzip it to get `app-debug.apk`. Copy that to the tablet and install
   (allow "install unknown apps" when prompted).

## Option C — build locally with Android Studio
Install Android Studio (free), **Open** this folder, let it sync, then
**Build → Build APK(s)**. The file is created at:
    app/build/outputs/apk/debug/app-debug.apk
(For a release-signed one, see README.md.)
