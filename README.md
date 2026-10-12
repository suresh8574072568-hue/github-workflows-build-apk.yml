# github-workflows-build-apk.yml

name: Build Vote Challenge APK

on:
  workflow_dispatch:
  push:
    paths:
      - 'VoteChallenge-AndroidStudio-Project.zip'
      - '.github/workflows/build-apk.yml'

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Extract Android project
        run: unzip -q VoteChallenge-AndroidStudio-Project.zip -d .

      - name: Set up Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Build debug APK
        working-directory: VoteChallenge
        run: gradle assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: VoteChallenge-APK
          path: VoteChallenge/app/build/outputs/apk/debug/app-debug.apk
          if-no-files-found: error
          
