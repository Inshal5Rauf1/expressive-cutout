# Contributing

This is a personal, hardened fork of Expressive Cutout. Changes are welcome as issues or
pull requests against the `hardened` branch on GitHub.

## Ground rules

- No network code, ever. The app declares no `INTERNET` permission, and the CI guard
  fails any merge that adds it, adds `READ_PHONE_STATE`, turns `allowBackup` back on, or
  lets the accessibility service read window content again.
- No analytics, crash reporting or ads SDKs.
- Keep patches small. The fork merges upstream `dev` regularly, so every change here is
  one more thing to carry across a merge.
- Build before you claim a change works: `./gradlew assembleDebug`.

## Code style

See [docs/CODING_STYLE.md](docs/CODING_STYLE.md) and [AGENTS.md](AGENTS.md). In short:
KDoc above every declaration, source grouped by responsibility, dependencies declared
once in `gradle/libs.versions.toml`.

## Licence

GPLv3. By contributing you agree that your changes are released under the same licence.
