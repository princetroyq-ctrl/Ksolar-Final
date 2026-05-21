name: Build K-Solar Android APK

# Trigger: manual run from GitHub Actions tab OR push to main
on:
  workflow_dispatch:
  push:
    branches: [main]
    paths:
      - 'android-app/**'
      - '.github/workflows/build-android.yml'

jobs:
  build:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: ./android-app

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          distribution: 'temurin'
          java-version: '17'

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Set up Android SDK
        uses: android-actions/setup-android@v3

      - name: Install Capacitor dependencies
        run: npm install

      - name: Add Android platform
        run: npx cap add android || echo "Android platform already added"

      - name: Copy app icons into Android project
        run: |
          for density in mdpi hdpi xhdpi xxhdpi xxxhdpi; do
            cp -f android-resources/mipmap-$density/ic_launcher.png \
              android/app/src/main/res/mipmap-$density/ic_launcher.png
            cp -f android-resources/mipmap-$density/ic_launcher_round.png \
              android/app/src/main/res/mipmap-$density/ic_launcher_round.png
            cp -f android-resources/mipmap-$density/ic_launcher_foreground.png \
              android/app/src/main/res/mipmap-$density/ic_launcher_foreground.png 2>/dev/null || true
          done

      - name: Copy splash screen
        run: |
          mkdir -p android/app/src/main/res/drawable
          cp -f android-resources/splash.png android/app/src/main/res/drawable/splash.png

      - name: Sync Capacitor config
        run: npx cap sync android

      - name: Build APK (debug)
        run: |
          cd android
          chmod +x ./gradlew
          ./gradlew assembleDebug

      - name: Upload APK as artifact
        uses: actions/upload-artifact@v4
        with:
          name: k-solar-debug-apk
          path: android-app/android/app/build/outputs/apk/debug/app-debug.apk
          retention-days: 30
