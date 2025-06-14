# Audio Processing

A sophisticated Python tool for splitting audio files based on silence detection, optimized for speech transcription and LLM processing.

## Features

- **Intelligent Splitting**: Split audio files on silence gaps (25+ seconds by default)
- **Format Support**: Process MP3, WAV, M4A, FLAC, OGG, AAC, MP4
- **FLAC Optimization**: Convert to 16kHz mono FLAC for optimal transcription
- **Batch Processing**: Process entire directories with progress tracking
- **Smart File Management**: Automatically organize processed files
- **Rich CLI**: Modern command-line interface with interactive configuration
- **Metadata Generation**: Comprehensive processing reports and statistics

## Installation

This project uses [uv](https://docs.astral.sh/uv/) for dependency management.

```bash
# Install dependencies
uv sync
```

## Usage

### Quick Start

1. Place your audio files in the `input/` directory
2. Run with the modern CLI:

```bash
# Interactive mode (recommended for first-time use)
uv run python audio_splitter_cli.py process --interactive

# Default processing
uv run python audio_splitter_cli.py process

# Keep files in place (don't move to processed/)
uv run python audio_splitter_cli.py process --no-move

# Show configuration without processing
uv run python audio_splitter_cli.py process --dry-run
```

### Alternative Commands

```bash
# Simple batch processing
uv run python run.py

# Single file optimization
uv run python audio_splitter_cli.py optimize input.wav -o output.flac

# Analyze audio file
uv run python audio_splitter_cli.py analyze input.wav
```

Split files will be saved to `output/[filename]/` directories. Successfully processed files are moved to `input/processed/` by default.

## How It Works

The tool uses advanced audio processing libraries to:
- Detect silence gaps longer than 25 seconds using Librosa
- Split audio at these points while preserving context
- Keep 1 second of silence at chunk boundaries
- Filter out chunks shorter than 5 seconds
- Optimize output as 16kHz mono FLAC for transcription
- Generate comprehensive metadata and reports
- Organize processed files automatically

## Configuration

### Default Settings
- **Silence Detection**: 25 seconds minimum gap
- **Silence Threshold**: 0.01 amplitude (adjustable)
- **Chunk Boundaries**: 1 second silence preserved
- **Output Format**: FLAC (16kHz, mono, compression level 8)
- **File Management**: Auto-move to `input/processed/`

### Configuration Methods
1. **Interactive**: `--interactive` flag for guided setup
2. **Config File**: Use YAML/JSON with `--config file.yaml`
3. **CLI Flags**: Override individual settings
4. **Defaults**: Optimized for speech transcription

### Example Configuration File
```yaml
target_sample_rate: 16000
flac_compression_level: 8
min_silence_duration: 25.0
min_chunk_duration: 5.0
move_processed: true
```

## Requirements

- Python 3.11+
- PyDub (for audio processing)
- FFmpeg (for various audio format support)

## Project Structure

```
audio_processing/
   input/          # Place audio files here
   output/         # Split files saved here
   audio_splitter.py  # Main processing logic
   run.py          # Convenience runner
   main.py         # Basic entry point
```