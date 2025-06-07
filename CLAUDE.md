# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an audio processing tool that splits monologue audio recordings (containing speech and/or music) into chunks based on silence detection. The core algorithm detects 25-second silence gaps and splits audio files at these boundaries.

**Critical Silence Detection Rule**: When 25 seconds of silence is detected, the chunk boundary is placed at the START of the silence period (end of previous chunk) and the next chunk begins at the END of the silence period. This preserves the 25-second silence gap between chunks.

## Core Architecture

- `audio_splitter.py`: Main processing logic with silence detection using PyDub
- `run.py`: Convenience entry point that imports and runs the main function
- `main.py`: Basic entry point (currently minimal)
- `input/`: Directory for source audio files
- `output/`: Directory for split audio chunks, organized by source filename

## Key Processing Parameters

- **min_silence_len**: 25000ms (25 seconds) - threshold for chunk boundaries
- **silence_thresh**: -40dB - amplitude threshold for silence detection
- **keep_silence**: 1000ms (1 second) - silence preserved at chunk boundaries
- **min_chunk_duration**: 5000ms (5 seconds) - chunks shorter than this are discarded
- **output_format**: WAV files for consistency

## Development Commands

```bash
# Install dependencies
uv sync

# Run the audio splitter
uv run python run.py

# Direct execution
uv run python audio_splitter.py
```

## Supported Audio Formats

Input: MP3, WAV, M4A, FLAC, OGG, AAC, MP4
Output: WAV format (standardized)

## Dependencies

- **PyDub**: Audio processing and silence detection
- **tqdm**: Progress bars for batch processing
- **FFmpeg**: Required system dependency for various audio format support

## Processing Workflow

1. **File Discovery**: Scan input directory for supported audio formats
2. **Audio Loading**: Load files using PyDub with timing and size reporting
3. **Silence Analysis**: Detect silence gaps using split_on_silence()
4. **Chunk Filtering**: Remove chunks shorter than 5 seconds
5. **Export**: Save valid chunks as numbered WAV files in subdirectories

## Output Structure

```
output/
├── audio_file1/
│   ├── split01.wav
│   ├── split02.wav
│   └── ...
└── audio_file2/
    ├── split01.wav
    └── ...
```

## Development Notes

**Progress Reporting Improvements Needed**: The current implementation should be enhanced with better stdout feedback showing progress during:
- Audio file loading and analysis phases
- Silence detection processing (currently has basic progress bar)
- Chunk filtering and validation steps
- Export operations with file-by-file progress
- Overall batch processing status across multiple files
- Memory usage and processing speed metrics

Consider adding more granular progress indicators and ETA calculations for long-running operations.