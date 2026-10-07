# Rickroll Bash

A tiny Ubuntu/Bash terminal project that plays the recognizable melody of **“Never Gonna Give You Up” by Rick Astley** using generated sine waves and the `SoX` command-line audio tool.

No audio file is required—the melody is generated note-by-note directly from the terminal. 🎶

## Features

* Generates the melody using sine waves
* Designed for Ubuntu/Linux
* Runs entirely from the Bash terminal
* Uses `SoX` (`play`) for audio synthesis
* No MP3/WAV file required
* Perfect for a lightweight terminal rickroll

## Requirements

* Ubuntu or another Linux distribution with Bash
* `SoX`
* `bc`

Install the required packages:

```bash
sudo apt update
sudo apt install sox bc
```

> **Note:** `SoX` provides the `play` command used to generate the tones. `bc` is used for converting the note durations from milliseconds to seconds.

## Installation

Clone or download the project, then enter its directory:

```bash
cd rickroll-bash
```

Make the script executable:

```bash
chmod +x rickroll.sh
```

## ▶Usage

Run:

```bash
./rickroll.sh
```

The script will generate and play the melody through your default audio output.

You can also run it directly with Bash:

```bash
bash rickroll.sh
```

## How It Works

The melody is encoded as a sequence of:

```text
frequency:duration
```

For example:

```text
440:132
494:132
587:132
494:132
```

The first number represents the frequency of the note in **Hz**, while the second represents its duration in **milliseconds**.

The script extracts these values:

```bash
f=${n%:*}
d=$(printf "%.3f" "$(echo "${n#*:}/1000" | bc -l)")
```

Then it generates a sine wave using SoX:

```bash
play -nq synth "$d" sine "$f" fade h 0.01 "$d" 0.01
```

For rests, the frequency is represented by `0`:

```text
0:40
```

The script detects these and pauses instead of generating a tone:

```bash
if [ "$f" -eq 0 ]; then
    sleep "$d"
else
    play ...
fi
```

### Signal Generation

The project uses a simple audio-synthesis approach:

```text
Note
 ↓
Frequency + Duration
 ↓
Bash parses values
 ↓
SoX generates sine wave
 ↓
Audio output
```

This produces a softer synthesized tone compared with using the traditional PC-speaker `beep` command.

## Project Structure

```text
rickroll-bash/
├── rickroll.sh
└── README.md
```

## Customizing the Melody

You can replace the contents of the `song` variable with your own sequence of notes.

Format:

```text
FREQUENCY:DURATION
```

Example:

```bash
song="440:500 494:500 523:500"
```

This generates three tones:

```text
440 Hz  → 500 ms
494 Hz  → 500 ms
523 Hz  → 500 ms
```

To add a pause:

```text
0:500
```

which produces a 500 ms silence.

## About the Melody

The included note sequence is based on the melody of **“Never Gonna Give You Up”**, performed by Rick Astley.

The project intentionally uses synthesized tones rather than distributing an audio recording.

## Troubleshooting

### `play: command not found`

Install SoX:

```bash
sudo apt install sox
```

### `bc: command not found`

Install `bc`:

```bash
sudo apt install bc
```

### No sound

Check your system's audio output and try:

```bash
play -n synth 1 sine 440
```

If that produces a tone, SoX is working correctly.

### Permission denied

Make the script executable:

```bash
chmod +x rickroll.sh
```

Then:

```bash
./rickroll.sh
```

## Security

The script:

* Does not require root privileges to run
* Does not access the network
* Does not download files
* Does not collect data
* Does not modify system configuration

The only privileged command is the package installation:

```bash
sudo apt install sox bc
```

## License

This project is provided for **educational, experimental, and entertainment purposes**.

The melody is based on the copyrighted song **“Never Gonna Give You Up” by Rick Astley**. This repository does not distribute the original recording or lyrics.

## Purpose

This project demonstrates several simple but useful Linux concepts:

* Bash scripting
* Shell variables
* String manipulation
* Loops
* Conditional statements
* Command-line audio synthesis
* Frequency and duration representation
* Basic signal generation

A simple script can turn the Linux terminal into a surprisingly effective musical instrument.

---

**You know the rules, and so do I.** 🎵
