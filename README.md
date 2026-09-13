# FIR-Compare

Compare FIR filters in your browser. Inspect gain, timing and impulse responses side by side, or add a measured impulse response and target curve to predict the corrected frequency response.

FIR-Compare is a standalone desktop tool with a local browser interface. It produces interactive HTML reports that you can save, share and open offline, with optional numerical JSON export.

This repository hosts public downloads and documentation. Application source code is not published in this repository.

## Download

Download the package for your operating system and processor from [Releases](https://github.com/VilhoValittu/FIR-Compare/releases).

| Platform | Package name starts with |
| --- | --- |
| Windows x64 | `FIR-Compare-windows-x86_64` |
| Linux x64 | `FIR-Compare-linux-x86_64` |
| Linux ARM64 | `FIR-Compare-linux-arm64` |
| macOS Apple Silicon | `FIR-Compare-macos-arm64` |

Extract the entire `.7z` archive before launching. Each package includes the executable and license documents. You need a web browser and an archive extractor that supports `.7z`.

## Start the app

**Windows:** double-click `FIR-Compare.exe` in the extracted folder.

**Linux and macOS:** open a terminal in the extracted folder and run:

```sh
./FIR-Compare
```

If execution fails with a permission error, run `chmod +x FIR-Compare`, then try again. The macOS download is a terminal executable for Apple Silicon, without signing or notarization.

The app opens its local address in your default browser. If the browser does not open automatically, copy the address printed in the terminal into your browser.

## Compare filters

1. Add one or more FIR WAV files. You can add files from different folders; the list shows sample rate, channel count, sample count and duration.
2. Select the channel to analyze. Channel numbering starts at **1**; the FIR channel selection applies to every selected filter.
3. Optionally add both a **measured impulse response WAV** and a **target curve TXT** to include the predicted corrected response and target error.
4. Click **Compare filters**.

Measurement input must be an impulse response, not a music recording or an undeconvolved measurement sweep. Measurement and target must be supplied together, or both omitted.

The target file contains ascending frequency/magnitude pairs in Hz and dB, separated by whitespace. Blank lines and comments beginning with `#` are accepted. For example:

```text
# Frequency_Hz Magnitude_dB
20 4.0
100 3.0
1000 0.0
20000 -4.0
```

This is a format example, not a recommended target curve.

## Explore the report

- Compare FIR magnitude, group delay and impulse responses, plus predicted frequency response when measurement and target are supplied.
- Select a reference FIR for the side-by-side metrics table and magnitude/group-delay difference plots. Difference plots require at least two FIRs.
- Switch between raw and relative magnitude, choose metric groups and hide or show individual curves.
- Adjust frequency ranges and plot axes, and inspect values with pointer readouts.
- Choose a light or dark theme in both the app and saved reports.

Open **Analysis notes and detailed metrics** for analysis explanations and **About this plot** for plot-specific guidance. Numerical differences describe the filters; they are not an overall sound-quality ranking.

### Interpreting the results

FIR coefficients retain their native sample rates and are never resampled. Source FIRs, measurements and targets are read without modification. Files at different sample rates can be compared; frequency comparisons are limited by the available data.

Relative boost, cut and power ratios use mean FIR gain in **500–2000 Hz** as their reference. That band may be a stopband for a subwoofer or low-pass filter, so a large relative boost does not necessarily mean positive raw gain. Check the raw magnitude view and raw peak gain as well.

The group-delay view with bulk delay removed subtracts the high-frequency median delay. It is not a minimum-phase reconstruction or a verdict on audible quality. A predicted corrected response is a calculation from the supplied inputs, not a new acoustic measurement.

## Saved reports and closing the app

By default, reports are saved under `Documents/DecayCore/Analyze` in your home directory. HTML reports contain their own data and scripts, so they remain interactive offline after the app closes.

**Cancel comparison** stops the current analysis. **New comparison** returns to your retained file selections. Use **Close app** in the upper-right corner to stop the local server, including from the report view. The server also shuts down after ten minutes without requests; closing a browser tab does not immediately stop it.

Analysis runs locally on your computer through a server bound to `127.0.0.1`. No account or cloud upload is needed.

## Command-line use

Run these commands from the extracted folder. On Windows PowerShell, replace `./FIR-Compare` with `.\FIR-Compare.exe`.

Compare FIR files and save an HTML report:

```sh
./FIR-Compare --filters a.wav b.wav --output comparison.html
```

Include a measurement and target, then open the report:

```sh
./FIR-Compare measurement.wav --target target.txt --filters a.wav b.wav --open
```

Analyze the second channel of a stereo FIR and also export numerical JSON:

```sh
./FIR-Compare --filters stereo.wav --filter-channel 2 --json analysis.json
```

`--channel` selects the measurement channel. Both channel options default to `1`. Omit `--output` to use the default report folder. `--no-browser` starts the file picker and prints its address without opening a browser. Run `./FIR-Compare --help` for all options.

## Input limits

A comparison accepts up to **32 FIRs**, **512 MiB** of combined input files and a **4 MiB** target TXT file. An estimated analysis-memory budget of **1024 MiB** also applies, so large workloads may be rejected before reaching the file-count or input-size limits. This budget is an estimate, not a fixed limit on process memory.

## Feedback and licenses

Report problems or request improvements through [GitHub Issues](https://github.com/VilhoValittu/FIR-Compare/issues). Include your FIR-Compare version, operating system, steps to reproduce and any error message. For input-related problems, include sample rates, channel counts and filter lengths.

See the `LICENSE` and `THIRD_PARTY_LICENSES.txt` included with your download for license information and third-party notices.
