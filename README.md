name: Build APK
on:
  push:
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - if: hashFiles('project.zip') != ''
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - if: hashFiles('project.zip') != ''
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: 8.7
      - if: hashFiles('project.zip') != ''
        run: |
          unzip -q project.zip -d src_unzipped
          cd src_unzipped
          gradle assembleDebug
      - if: hashFiles('project.zip') != ''
        uses: actions/upload-artifact@v4
        with:
          name: GameBoost-APK
          path: src_unzipped/app/build/outputs/apk/debug/*.apk
