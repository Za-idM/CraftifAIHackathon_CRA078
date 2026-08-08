# Privacy Auto-Redaction Pipeline

Built with PipeGen (CraftifAI) — Embedded Hardware + AI Buildathon, Track A (Perception).

## What it does
Detects people in a video file and automatically blurs them, producing a privacy-safe output video. Useful for dashcam, CCTV, or interview footage that needs to be shared without revealing identities.

## Pipeline
Video file -> YOLOv8m detection (TensorRT FP16) -> custom Gaussian blur on detected regions -> blurred MP4 + detections JSON

Built entirely through PipeGen's requirement conversation, PIR review/approval, and compile/run workflow.

## Outputs
- outputs/blurred_video.mp4 - redacted output
- outputs/detections.json - per-frame detections
- input/source_video.mp4 - original source clip

## Limitations
Person-level (not face-only) redaction. Confidence threshold may miss small/distant people.
