## Cursor Cloud specific instructions

### Project overview

Ackpine is a multi-module Android library for installing/uninstalling apps. It is a Gradle (Kotlin DSL) project — no backend services, Docker, or external dependencies are needed. See `README.md` and `docs/building.md` for full documentation.

### Environment requirements

- **JDK 21** (pre-installed in the Cloud VM)
- **Android SDK** at `/opt/android-sdk` with `platforms;android-35` and `build-tools;35.0.1`
- Environment variables `ANDROID_HOME=/opt/android-sdk` and `JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64` must be set (configured in `~/.bashrc`)

### Key Gradle tasks

| Task | Purpose |
|---|---|
| `./gradlew :buildAckpine` | Build all library modules (release AARs) |
| `./gradlew apiCheck` | Validate binary API compatibility (the project's "lint" check) |
| `./gradlew apiDump` | Regenerate API dump files after intentional API changes |
| `./gradlew :sample-java:assembleDebug` | Build Java sample APK |
| `./gradlew :sample-ktx:assembleDebug` | Build Kotlin sample APK |
| `./gradlew :sample-api34:assembleDebug` | Build API 34+ sample APK |

### Gotchas

- **No automated tests**: This project has no unit/integration test suites. Validation is done via `apiCheck` and successful builds.
- **`./gradlew lint` fails** with an upstream AGP bug (ApiDetector NPE with K2 UAST). This is a known issue and lint is not part of CI — do not try to fix it.
- **No `package.json`, `requirements.txt`, or similar** — all dependencies are managed by Gradle and downloaded automatically.
- **Release sample builds** require a `keystore.properties` file (see `docs/building.md`). Debug builds work without it.
- **Gradle configuration cache** is enabled by default. After modifying build-logic, you may need `--no-configuration-cache` on the first build.
