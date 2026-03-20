# aGate

A minimal noise gate VST3 plugin built with [JUCE](https://juce.com/) 8.

## What it does

aGate silences audio that falls below a configurable threshold level. When the input signal is quieter than the threshold, the output is muted — useful for cutting background noise or bleed between musical phrases.

## Parameters

| Parameter | Range | Default | Description |
|-----------|-------|---------|-------------|
| Threshold | 0.0 – 1.0 (linear) | 1.0 (0 dB) | Signals below this amplitude are silenced. Displayed in dBFS. |

## Current state

- **Working:** Basic gate logic — samples below the threshold are zeroed out per-channel.
- **No GUI:** The plugin uses the host's generic parameter UI (`hasEditor()` returns `false`).
- **No attack/release:** The gate switches hard on/off with no smoothing, which can produce clicks at the gate boundary. A Python proof-of-concept in `concept/aGain.py` demonstrates attack/release smoothing that has not yet been ported to the C++ processor.
- **No state persistence:** `getStateInformation` / `setStateInformation` are stubs; the threshold is not saved with the session.
- **Format:** VST3 only, stereo in/out (mono also supported).

## Building

### Requirements

- CMake 3.30+
- C++23-compatible compiler
- On Linux: `libxext-dev libxrandr-dev libxinerama-dev libxcursor-dev libxi-dev libgtk-3-dev libasound2-dev`

JUCE 8.0.8 is fetched automatically via CMake `FetchContent`.

### Steps

```sh
cmake -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release
```

The built VST3 will be placed in the `build/` directory.

## CI

GitLab CI builds the plugin on every push using a custom Docker image (`ghcr.io/anothersimplecoder/cmake:latest`) on an `amd64` runner.

## Project layout

```
aGate/
├── CMakeLists.txt          # Build configuration, JUCE dependency
├── include/
│   └── AGateProcessor.hh   # Plugin processor class declaration
├── src/
│   └── AGateProcessor.cc   # Gate logic and JUCE plugin boilerplate
└── concept/
    └── aGain.py            # Python prototype with attack/release smoothing
```
