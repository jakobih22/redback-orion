# Ball Detection Pilot — Current Status

## Purpose

This work investigates a dedicated AFL ball detector before ball tracking and
trajectory generation are added to Project Orion. It remains an experimental
pilot and does not modify the active player-tracking service.

## Work completed

- Established a baseline showing that the original all-classes model produced
  no BALL detections on a fixed 20-second CAR–GCS clip.
- Audited the existing dataset and found only 29 BALL annotations.
- Defined consistent rules for visible, blurred, distant, held and partly
  occluded balls, as well as negative and uncertain examples.
- Built and evaluated several versioned ball-only datasets and models.
- Expanded Dataset V4 to 206 reviewed images: 187 training images and 19
  preserved validation images.
- Coordinated a beginner-friendly video scouting task with another team member,
  then reviewed and labelled the resulting candidate frames.
- Compared V3 and V4 on unchanged validation images and fixed video clips.
- Tested standard ByteTrack, BoT-SORT and a tuned ByteTrack configuration.

## Current result

V4 improved fixed-set precision from 0.4766 to 0.5453 and mAP50 from 0.4972
to 0.5217. Visual review found that V4 followed the red ball more often and
gave cleaner high-confidence behaviour on the reviewed yellow-ball clip.

The detector is not finished. It can still lose the ball during fast or
occluded movement and can sometimes confuse shoes, clothing, referees or crowd
objects with the ball. The validation set is small, and the reviewed videos are
development or same-match tests rather than fully independent match evidence.

Standard trackers did not maintain one reliable ball identity. Lower tracking
thresholds admitted more false detections, while higher thresholds caused the
real ball track to break. Tracking therefore remains a separate next phase.

## Documentation

- [`ball_detection_pilot.md`](ball_detection_pilot.md) — initial baseline and
  first labelling pilot.
- [`ball_labeling_guidelines.md`](ball_labeling_guidelines.md) — annotation and
  quality-review rules.
- [`ball_detection_v4_experiment.md`](ball_detection_v4_experiment.md) —
  Dataset V4 training and evaluation evidence.
- [`../configs/bytetrack_ball.yaml`](../configs/bytetrack_ball.yaml) — tuned
  experimental tracker configuration.

## Evidence storage

Full match videos, labelled datasets, Label Studio exports, model weights,
training runs and annotated evidence videos are intentionally excluded from
Git because they are large experimental artifacts. The experiment report
records dataset counts, settings, comparisons and important limitations.

## Next phase

1. Test V4 on a completely unused match before using any of its frames for
   training.
2. Record missed-ball and false-positive timestamps.
3. Prototype a ball-specific temporal tracker that can bridge short detection
   gaps and reject stationary or implausible false detections.
4. Compare that tracker with the ByteTrack and BoT-SORT baselines.
5. Keep the pilot separate from the active Orion service until team review.
