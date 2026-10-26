# Ghost

Ghost is an Electron desktop app for **interview practice and learning**. It can transcribe audio, identify likely questions, stream AI-generated explanations, and analyze a screenshot of a coding or system-design problem.

> [!IMPORTANT]
> **Educational use only. This project is not for cheating. Do not use it during a live interview, exam, or assessment, or to misrepresent your skills or work.** Use it to study, practice, and understand example answers. Get consent before capturing or transcribing anyone else's audio, and follow applicable laws, platform rules, and OpenAI's terms. AI output can be incomplete or incorrect—verify it and learn the underlying material.

## Star or fork the project

This repository is currently private. Only people with access can view it; make it public in GitHub repository settings before expecting the general public to star or fork it.

- **Star it:** Visit [Nikayel/interview-copilot](https://github.com/Nikayel/interview-copilot) and click **Star** to bookmark the project and show your support.
- **Fork it:** Open the repository and click **Fork**. GitHub will create a copy under your account.
- **Clone your fork:**

  ```bash
  git clone https://github.com/<your-username>/interview-copilot.git
  cd interview-copilot
  ```

  Replace `<your-username>` with your GitHub username.

## How the app works

1. **Choose an audio source.** Start capture with a microphone or, where supported and permitted, system audio. The app needs the relevant operating-system permissions.
2. **Transcribe speech.** Non-silent audio is buffered locally and sent to the OpenAI transcription API when a segment has at least about one second of audio and one second of silence has passed; a 20-second limit also triggers processing. The current transcription model is `gpt-4o-mini-transcribe`.
3. **Detect questions and generate responses.** The app uses question-detection logic to identify likely questions, then sends the detected text to the selected OpenAI chat model. The response streams into the app as it is generated.
4. **Analyze a problem screenshot (optional).** Use the screenshot action to capture another desktop window. The app sends the image to the selected vision-capable model and displays its analysis.
5. **Review settings and history.** Add optional background/resume context, choose a model, adjust window appearance, and review or clear saved answer history.

### Data and privacy

- Audio is sent to OpenAI for transcription; detected question text and optional context are sent to the selected OpenAI model. A screenshot is sent when you use screenshot analysis.
- The app saves settings and up to 100 history entries in Electron's local user-data folder. It uses Electron `safeStorage` to encrypt the API key when the operating system supports it; when encryption is unavailable, the key may be stored unencrypted in the local settings file.
- Window content protection is enabled in the app, but its behavior depends on the operating system and other software. **It is not a privacy or screen-sharing guarantee.** Do not rely on it to hide activity.

## Features

- Microphone capture and system-audio capture where supported
- Speech transcription and question detection
- Streaming AI-generated practice answers
- Optional screenshot analysis for coding and system-design practice
- Optional resume/background context
- Local answer history, with controls to delete entries or clear history
- Always-on-top and window-opacity settings
- Electron, React, and TypeScript desktop app for Windows, macOS, and Linux (permissions and capabilities vary by platform)

## Requirements

- Node.js **20.19+** or **22.12+**
- npm (included with Node.js)
- An OpenAI API key and API access to the models you choose

## Run locally

```bash
git clone https://github.com/Nikayel/interview-copilot.git
cd interview-copilot
npm install
npm run dev
```

When the app opens, go to **Settings** and enter your OpenAI API key. You can optionally choose a model and add background context there.

For local development, you can also put the key in a root-level `.env` file:

```dotenv
OPENAI_API_KEY=your_openai_api_key
```

Do not commit `.env` or share your API key. The repository's `.gitignore` excludes `.env`.

## Models and settings

The app defaults to `gpt-6-luna`. The model selector includes `gpt-5.6-luna`, `gpt-5.6-terra`, and `gpt-4o-mini` when available to your API account. The transcription model is configured separately as `gpt-4o-mini-transcribe`.

| Setting | What it does | Default |
| --- | --- | --- |
| OpenAI API key | Authenticates transcription and model requests | Required |
| OpenAI model | Generates practice answers and screenshot analysis | `gpt-6-luna` |
| Resume/background context | Optional context for answer generation | Empty |
| Pause threshold | Displayed and saved in Settings; currently not applied to audio processing | 1500 ms |
| Window opacity | Adjusts the app window's opacity | 100% |
| Always on top | Keeps the app window above other windows | Enabled |

Model availability, features, and API costs depend on your OpenAI account. Review your provider's current documentation and pricing before use.

## Development commands

```bash
npm run dev          # Start the app in development mode
npm run typecheck    # Type-check the main and renderer processes
npm run lint         # Run ESLint
npm run build        # Type-check and build the app
```

## Build packages

Run the platform-specific command on the operating system for which you want to build:

```bash
npm run build:win    # Windows
npm run build:mac    # macOS
npm run build:linux  # Linux
```

Packaged output is written to `dist/`. Native build tools and signing/notarization may be needed for some platforms and distribution methods.

## Platform notes

- **Windows:** Grant microphone access in Windows privacy settings. Screen-capture protection behavior depends on the capture application and operating-system support.
- **macOS:** Grant microphone and screen-recording permissions as prompted. System-audio capture support depends on the macOS version and capture source.
- **Linux:** Audio capture depends on the desktop environment and audio stack (such as PulseAudio or PipeWire). Check platform permissions and dependencies if capture is unavailable.

## Contributing

Contributions that improve learning, accessibility, reliability, or documentation are welcome. Please keep the educational-use disclaimer intact.

1. Fork the repository and clone your fork (see [Star or fork the project](#star-or-fork-the-project)).
2. Create a branch for your change:

   ```bash
   git checkout -b your-change
   ```

3. Install dependencies and make your changes:

   ```bash
   npm install
   ```

4. Run the relevant checks:

   ```bash
   npm run typecheck
   npm run lint
   npm run build
   ```

5. Commit your changes, push the branch to your fork, and open a pull request against `Nikayel/interview-copilot`.

Please do not include API keys, personal interview recordings, screenshots, or other sensitive data in commits or issues.

## Project structure

```text
.
├── build/                    # Platform icons and macOS entitlements
├── resources/                # App icon assets
├── electron-builder.yml      # Desktop packaging configuration
├── electron.vite.config.ts   # Main, preload, and renderer build configuration
├── package.json              # Dependencies and npm scripts
├── pnpm-lock.yaml            # pnpm lockfile
└── src/
    ├── main/                 # Electron window, IPC, and application services
    │   ├── ipc/              # Renderer-to-main process handlers
    │   └── services/         # OpenAI, transcription, detection, history, settings, screenshots
    ├── preload/              # Typed, restricted bridge between Electron and the UI
    └── renderer/src/
        ├── components/       # React interface
        ├── hooks/            # Renderer hooks
        ├── services/         # Microphone and system-audio capture
        └── store/            # Zustand application state
```

## License

This repository does not currently include a license file. Please check with the project owner before redistributing or reusing the code beyond what GitHub's terms permit.
