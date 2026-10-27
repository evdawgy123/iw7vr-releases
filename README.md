# IW7VR Launcher v0.1.0 — preview candidate

Run IW7VR-Launcher.exe on Windows x64 (.NET Framework4.8). Build: source/build.ps1.
The launcher has NOT been installed and has not modified the real game. Its
bundled mod payload is v0.8.4 IKStableOneHand, not the untested v0.9.0 WeaponTest.
Only running this EXE opens the UI; install/restore happen when you click them.

Finds Steam from the Windows registry and libraryfolders.vdf, scans app manifests,
and confirms iw7_ship.exe. Browse supports a custom Steam library. Install copies
the proxy and loader and fetches the pinned iw7-mod.exe from GitHub Releases;
hash-checks the download before changing the game folder; preserves
existing values; adds missing schema defaults; creates an original restore point
and a per-operation rollback. Updates retain that FIRST original restore point.
Uninstall verifies backups before restoring and retains current configuration in
LocalAppData/IW7VR for later installs. Closed-game guards prevent DLL replacement
while IW7 runs. Install/restore request Windows elevation on an access denial.
The launch button runs iw7-mod from the selected folder. Choose offline Zombies;
the launcher does not enforce or enter a particular native menu/mode for you.

The settings screen is generated from ini-schema.json:101 documented native/trial
options, numeric ranges, enums, bool toggles and all11 gamepad button mappings.
Each section resets independently. Signed packages can also update the schema; new defaults are merged without replacing existing values. Edits save on Save settings; unknown INI keys
and comments are preserved. Some values take effect only after restarting IW7.
Gameplay DLLs are not changed by launcher UI or schema edits.

Community links are placeholders in launcher.config.json. Set YouTubeUrl,
TwitchUrl and DiscordUrl to HTTPS links. No outbound messages are sent.

## Signed mod release feed

FeedUrl/PublicKeyXml are deliberately unconfigured; AutoUpdate defaults false.
Configure your own HTTPS feed and trusted RSA public key in Updates, then enable
automatic checks. Only RSA-SHA256 signed release envelopes are accepted, with a
pinned2048-bit-or-larger public key. The signed payload contains Version,
DownloadUrl, Sha256 and Size. Downloads follow at most five HTTPS redirects,
check the signed size/hash, reject archive escapes/duplicate/unknown files,
verify the payload manifest, and refuse version rollback. The game must be closed.
This feed updates the MOD PAYLOAD. This preview does not replace its own running
launcher EXE; distribute launcher releases separately until self-update is added.
Live HTTPS feed delivery is UNTESTED because no feed/key has been provided.

Use ReleasePublisher.exe keygen PRIVATE.xml PUBLIC.xml. Keep PRIVATE.xml outside
all release packages. Paste PUBLIC.xml into launcher.config.json. Build a ZIP
containing payload/ and payload/payload.manifest.json, with only supported files.
Version in payload.manifest.json must equal the release version. Create RELEASE.json:
{"Version":"0.1.1","DownloadUrl":"https://YOUR_HOST/release.zip",
 "Sha256":"ZIP_SHA256","Size":ZIP_SIZE,"SourceBundleUrl":"https://YOUR_HOST/source.zip"}
Then ReleasePublisher.exe sign RELEASE.json PRIVATE.xml feed.json. Publish the
ZIP and feed.json on HTTPS hosting you control. Packages omit iw7-mod; the launcher
fetches it from upstream using the pin in launcher.config.json.
A signature authenticates your package; it is not an Authenticode certificate for
Windows SmartScreen. No signing key or certificate is bundled/generated for you.

## Verification and release status

24 assertions pass in an isolated temporary fixture: INI preservation/duplicates,
numeric validation, install and update original-backup retention, package hashes,
mid-copy rollback, corrupt-backup rejection, restore, preserved settings, owned
file restrictions, ZIP traversal rejection, valid and tampered signatures and
missing-key rejection. No real game install/launch was executed by these tests.
Screenshots were rendered from the actual Windows Forms app off-screen using
DrawToBitmap, then visually inspected; no user desktop was captured.

The official-loader/notices cleanup is included in this release-preparation package.
Before public release, review the provenance notices and read logs/license-audit.md and the cleanup report. iw7-mod is fetched directly
from upstream rather than redistributed.


## License-prepared package
This separate candidate includes the official loader, complete notices. iw7-mod is fetched from upstream at install time using launcher.config.json. Read LICENSE-CLEANUP.md and PROVENANCE.md. It is NOT INSTALLED. Gameplay remains the v0.8.4 IK candidate; no v0.9.0 trial is installed.

Signed update elevation re-verifies the signature, ZIP size/hash, payload version and anti-rollback state before privileged copies. The verified package can migrate ini-schema.json while retaining existing values.
