# Privacy Auto-Redaction Pipeline

Built with PipeGen (CraftifAI) — Embedded Hardware + AI Buildathon, Track A (Perception).

## What it does
Detects people in a video file and automatically blurs them, producing a privacy-safe output video. Useful for dashcam, CCTV, or interview footage that needs to be shared without revealing identities.

### Working , End-to-End
This isn't a mockup or a diagram — it's a real, compiled, executed pipeline with verifiable output.

Pipeline: Video file → YOLOv8m detection (TensorRT, FP16, compiled for RTX 4050) → custom Gaussian blur postprocess on every detected region → blurred MP4 + per-frame JSON detections.

Verified run stats (from the actual execution log):

-484 frames processed in ~3.14s wall-clock (~154 FPS end-to-end)
-484 detections written to detections.json, averaging 1.0 detection/frame
-Clean pipeline exit (EOS), no crashes
-TensorRT engine deserialized and ran without rebuild — fast, repeatable startup

## Pipeline
Video file -> YOLOv8m detection (TensorRT FP16) -> custom Gaussian blur on detected regions -> blurred MP4 + detections JSON

Built entirely through PipeGen's requirement conversation, PIR review/approval, and compile/run workflow.

## Outputs
- outputs/blurred_video.mp4 - redacted output
- outputs/detections.json - per-frame detections
- input/source_video.mp4 - original source clip

## Limitations
Person-level (not face-only) redaction. Confidence threshold may miss small/distant people.


### The problem
Every day, footage that could genuinely help people — a dashcam clip proving who was at fault in an accident, CCTV footage shared with a landlord, a recorded interview being published — sits on someone's drive unused, because sharing it means exposing the identity of a bystander who never consented to being filmed. The fix is usually "get someone to manually blur every frame in Premiere," which nobody has time for, especially not for a 10-second clip that isn't worth the effort — so the footage just never gets shared at all, and useful evidence or content stays locked away.

### The fix
This pipeline removes that manual step entirely. Feed it a video, and every person in every frame gets detected and blurred automatically — no editing software, no manual masking, no frame-by-frame tracking by hand.


## Screenshots — Before & After

| Original frame | Pipeline output |
|---|---|
| ![original](original_frame.png) | ![blurred](blurred.png) |


### Demo Video : https://drive.google.com/file/d/1Qg8mR14vf6fIZHqViSyWofI5M3HpX5XT/view?usp=sharing

### Real-World Application 

This solves a genuine, everyday friction point: footage that's useful but can't be shared because it identifies a bystander.

Dashcam footage — sharing proof of an incident (accident, near-miss, road rage) without exposing every pedestrian and driver caught in frame
CCTV / security footage — handing footage to a landlord, insurer, or law enforcement without manually redacting everyone except the subject in question
Interview & documentary footage — publishing content where background people didn't consent to appear
Compliance-driven redaction at scale — any organization currently paying someone to manually blur faces in Premiere or After Effects, frame by frame, for every clip they release

The pipeline takes any video file as input and requires zero manual intervention — point it at footage, get back a shareable, privacy-safe version.


### Real-World Impact 

Right now, the actual blocker to sharing sensitive footage usually isn't a technical one — it's that manual redaction doesn't scale. A 10-second clip isn't "worth" someone's time to hand-blur, so it just doesn't get shared, and useful evidence, content, or documentation stays locked away.

This pipeline removes that cost entirely. At ~154 FPS on a single consumer laptop GPU, redaction stops being a bottleneck — it becomes a one-command step in a footage-sharing workflow, at a speed and cost that makes "just blur it" the default instead of the exception.

The detector (YOLOv8m, TensorRT-compiled at FP16) finds the person in every frame, and a custom blur stage — generated and wired in by PipeGen as part of the compiled pipeline — redacts them before the frame ever reaches the output file. What used to be a manual, frame-by-frame editing task becomes a single automated pass: point it at a video, get back a privacy-safe one.
