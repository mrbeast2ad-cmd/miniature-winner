# MUQADAS AI – Android Companion (Phase 5)  — STATUS: UNTESTED

Nothing here has been compiled or run. No APK exists yet. Treat every feature as unverified until you build it and test it on your OPPO phone.

## Build (Android Studio)
1. Install Android Studio (Koala or newer) with JDK 17.
2. File → Open → select this `MuqadasAndroid` folder; accept the Gradle sync (it downloads the SDK 34 and dependencies).
3. Plug in the OPPO phone, enable Developer options → USB debugging, press Run.
   Fix any compile errors first; report them and I'll patch them.

## APK
- Debug APK: Build → Build Bundle(s)/APK(s) → Build APK(s); output in `app/build/outputs/apk/debug/`.
- Command line (after Studio created the Gradle wrapper): `./gradlew assembleDebug`.
- Release: Build → Generate Signed Bundle/APK, create a keystore and keep it private.
- Install on the phone: `adb install app-debug.apk`, or copy the file and allow "install unknown apps" for your file manager.
- OPPO note: ColorOS may restrict background activity; this app only works while open, which is by design.

## What exists
Voice (SpeechRecognizer + TextToSpeech, en-US / ur-PK), confirm-before-action dialog, open apps by name, open Settings screens, battery, note (share sheet), reminder (calendar insert), message *draft* (never auto-sent), Emergency STOP, encrypted token storage, HTTPS-only pairing.

## Known limits
- Reminders/notes/messages hand off to other apps; the app cannot see whether you saved or sent them.
- Urdu speech needs Google's Urdu recognition; Urdu TTS needs an installed Urdu voice.
- Placeholder launcher icon (system mic icon).
- Command parsing is keyword-based; anything else goes to the AI backend, which doesn't exist yet.

## Backend contract (to build in a later step; all HTTPS)
- `POST /api/devices/pair` {code, deviceName, platform} → {token}  (code shown once on the dashboard, short expiry, single use)
- `POST /api/chat` Bearer token {message, lang} → {reply}
- Planned: WebSocket `/ws/device` for command sync, per-device revocable tokens, signed command envelopes {id, deviceId, action, args, expires, nonce}, owner approval for sensitive actions. The same protocol is meant for the HP Windows 10 PC and laptop (`platform: "windows"`).
- Never accept commands from web content; only from the authenticated owner session.
