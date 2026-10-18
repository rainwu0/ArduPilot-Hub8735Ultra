# Contributing

繁體中文：[CONTRIBUTING.md](CONTRIBUTING.md)

Issues and pull requests are welcome here, in Chinese or English.

## Reporting problems

- Problems with an example's wiring, parameters, orientation or verification results: open an issue here, naming the example, the HAL repository tag or commit, and how you wired it.
- Problems with building, flashing or the HAL itself: report them in the [HAL repository](https://github.com/rainwu0/ArduPilot-RTL8735B).
- "I followed the example and it worked on my board" is worth reporting too.
- **Do not attach images or settings files that contain Wi-Fi credentials.** Before pasting boot messages, mask the `set ssid [...]` line; it prints the hotspot name.

## What an example contains

Each example is one directory named after its sensor set, for example `examples/bmi160-bmp280-qmc5883l/`:

```
examples/<example>/
├── README.md         sensor set, wiring table, build steps, parameter notes, board test summary
├── README.en.md      the same, in English
├── overlay.dat       hwdef overlay: sensors and mounting orientation
├── params/           load in the order of the file name numbers
│   ├── 10-board.parm          for any board: recommended failsafe values
│   ├── 20-use-<case>.parm     a use case, for example indoors, no GPS, no receiver
│   ├── 30-frame-<frame>.parm  a frame example, for example a quad X with brushless ESCs
│   └── 40-sensors.parm        settings this sensor set needs (leave out if none)
└── wiring.svg        wiring diagram (optional)
```

Sections of an example README:

1. **Sensor set**: models, interfaces, addresses, mounting orientation.
2. **Wiring**: which D number and header position each module pin goes to; 3.3 V and GND; how the board and the modules are placed.
3. **Build**: which HAL repository tag to use and the overlay command. No firmware images, because an image contains Wi-Fi credentials.
4. **Parameters**: what each parameter file changes and why, and what to change for your own frame.
5. **Board test**: for each check in the next section, what was done and what was measured, with the HAL repository tag and the date. A summary only.

The first line of `overlay.dat` and of each file in `params/` is `# SPDX-License-Identifier: GPL-3.0-or-later`.

## Checks an example must pass first

An example is accepted only after every check has been done:

1. **Boot and connection**: normal boot messages, Wi-Fi joined, and the ground station receives heartbeats and can read parameters.
2. **Sensors**: the IMU, the barometer (and the compass, if connected) are detected and pass the health checks, and the acceleration reads about 1 g at rest.
3. **Orientation**: with the front down and with the right side down (or lifted), pitch and roll have the right signs, and both axes are measured directly, not inferred. With a compass, turning the board 90° on a level surface changes the heading by about 90°.
4. **Failsafe**: ArduPilot SITL with the example's parameters loaded has gone through a ground station loss and landed.
5. **No private data**: the files contain no keys, Wi-Fi credentials, local paths, IP addresses or hotspot names.

## What is not accepted

- Firmware images.
- Wi-Fi firmware updates (OTA).
- Tuning for one particular vehicle, research data, and camera, video or NPU applications.

## Sending pull requests

- Commit messages are in English, in the form `area: summary`, for example `examples: add wiring for bmi160-bmp280-qmc5883l`.
- README, CONTRIBUTING, the CHANGELOG and every example README come in Traditional Chinese (Taiwan) and English: when you change one version, change the other in the same commit.
- When you add or change an example (its directory name, overlay, parameter files, wiring, or the HAL repository version it needs), add an entry under `[Unreleased]` in `CHANGELOG.md` and `CHANGELOG.en.md`, in the Keep a Changelog sections (Added, Changed, Fixed, Removed and so on).
- Cite only public sources that anyone can check, with a fixed commit or version. Mark inferences as inferences.
- Do not quote from vendor documents marked CONFIDENTIAL, even if they are publicly available: give their public path and a fixed commit, and let readers look them up.
- Contributions are released under the matching license in the README's "License" section.
