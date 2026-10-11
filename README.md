# SiteWatch Edge

SiteWatch Edge is an offline-first visual safety assistant for low-connectivity
workplaces. It uses OpenCV 5 for local image/video analysis, raises local alerts,
and queues privacy-minimised incident records for optional synchronization to AWS
after connectivity returns.

![SiteWatch Edge local interface](docs/images/local-ui-result.png)

## Competition target

- OpenCV AI Competition 2026, powered by AWS
- Internal submission deadline: 14 October 2026, 23:59 IST
- Official deadline: 26 October 2026, Pacific Time
- Entry status: build in progress; not yet submitted

## MVP scope

1. Read a webcam, video file, or RTSP stream locally.
2. Run an ONNX object detector through OpenCV 5 DNN.
3. Detect people and configured PPE classes.
4. Evaluate PPE and restricted-zone rules locally.
5. Show an on-screen warning and record a minimised incident event.
6. Queue events while offline and synchronize only approved metadata to AWS.

The MVP is an advisory system. It does not identify people, make employment
decisions, or replace site safety procedures.

## Licensing

The project is licensed under AGPL-3.0-only because the selected PPE model is an
AGPL-3.0 model. OpenCV Zoo YOLOX components are Apache-2.0. Model and evaluation
asset provenance is recorded in `models/MODEL_MANIFEST.json` and
`docs/DATA_MANIFEST.md`.

## Quick start

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements-lock.txt
python -m pip install -e . --no-deps
python scripts/fetch_models.py
python -m pytest
sitewatch simulate --events 3
```

Start the local interface:

```powershell
sitewatch-web --model models\buildvision-hazards-v0.3.onnx --port 7860
```

Then open `http://127.0.0.1:7860`. The server binds to the local machine by
default and does not upload images.

The OpenCV package is intentionally constrained to version 5 or later. If a
compatible wheel is unavailable, use the official OpenCV 5 build instructions
and then install this project with `--no-deps`.

## Repository layout

- `src/sitewatch_edge/`: local detection, rule evaluation, event queue, and CLI
- `tests/`: offline unit tests
- `infra/`: AWS SAM template and cloud receiver
- `docs/`: architecture, schedule, evidence, and submission checklist
- `artifacts/`: generated evaluation and submission evidence (kept out of Git)

## Current verification

See `docs/BUILD_STATUS.md`. No accuracy claim or competition claim should be
made until the corresponding evidence is recorded there.

![Offline-first architecture](docs/images/architecture.png)
