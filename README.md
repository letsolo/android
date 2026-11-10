# LetSolo Android

Signed APK downloads, update files, and release notes for LetSolo Android apps.

## Downloads

APK installers will be available in [Releases](https://github.com/letsolo/android/releases). Tags use `<application>-v<version>`. No application APKs have been published yet.

An APK is for direct device installation. An AAB is a Google Play submission artifact and is not published here as an installable APK. Google Play and TestFlight delivery run from private product repositories through the private shared workflows.

## Publication flow

The product repository owns the source, build, tests, and Android signing key. After uploading a signed APK artifact in its workflow run, it calls `letsolo/github-workflows/.github/workflows/release.yml@v1` with `platform: android`. Vapor creates a draft release here and attaches the APK, optional public update files, and `SHA256SUMS`.

The Google Play workflow separately accepts a signed AAB and uploads it to the internal testing track as a draft by default. Store credentials and signing material remain outside this public repository.

## Vapor authentication

All authors, committers, pushes, tags, and GitHub release publication use `letsolo-vapor[bot]`. Configure a new checkout first:

```bash
python3 -I scripts/bot.py --config <private-app-config> configure
```

Then ordinary `git commit` and `git push` use the bot. Use its local GitHub CLI alias:

```bash
git bot-gh release list
```

```bash
python3 -I scripts/bot.py verify
```

The App is installed across the organization; each local token is scoped to one allowed repository. Private App metadata stays outside the checkout. Credential errors stop operations rather than falling back to a personal login.

`Publisher identity` checks the commit identities, push actor, and tracked-file privacy. Actions uses variable `PUBLISHER_APP_CLIENT_ID` and encrypted secret `PUBLISHER_APP_PRIVATE_KEY`. Local hooks are a checkout safeguard, not server-side access enforcement.
