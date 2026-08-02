# QR Code Prediction (Deep Learning) — Early-Stage Experiment

This repo is an early, in-progress experiment exploring whether a neural network can
learn to "read" a QR code image and predict the number it encodes.

## Current status

**This is a data-generation / preprocessing prototype, not a working model yet.**
There is no model definition, training loop, or evaluation code in the notebook
at the time of writing — see [Limitations](#limitations) below.

## What's here

- **`qrcode/`** — 10,000 generated QR code images (`0.png` ... `9999.png`), each
  encoding the integer matching its filename. Generated with the `qrcode` Python
  package (version 1, error correction level `L`, box size 10, border 4).
- **`main.ipynb`** — the only code in the project. It currently:
  1. Generates the 10,000 QR code PNGs above (this cell is commented out since
     the images are already committed — it took ~1m 42s to run).
  2. Lists files in the `qrcode/` directory.
  3. Loads a QR code image with PIL, converts it to 1-bit black/white, resizes it
     to 29x29, and flattens it into a binary tensor — the intended input
     representation for a future model.
  4. Loops over the first 100 images, builds parallel lists `x` (flattened image
     tensors) and `y` (the encoded number) as a first pass at building a dataset.
- **`requirements.txt`** — a full `conda`/pip environment freeze from the original
  development machine (Windows, Python 3.10/3.11). Most of it is unrelated
  tooling (Jupyter, IPython, etc.); the actual dependencies used by the notebook
  are `qrcode`, `Pillow`, `torch`, and `torchvision`.

## Setup

```bash
pip install qrcode pillow torch torchvision
```

The full `requirements.txt` is an environment export rather than a curated
dependency list, so installing just the four packages above is enough to run
the notebook as it currently stands.

## Usage

Open `main.ipynb` and run the cells in order:

1. The QR-generation cell is commented out because `qrcode/` is already
   populated in this repo — uncomment it only if you want to regenerate the
   images.
2. The remaining cells load images from `qrcode/`, binarize/resize them, and
   build `x`/`y` tensor lists for the first 100 samples.

## Limitations

- No model (network architecture, loss function, optimizer, or training loop)
  has been written yet — the notebook stops after building `x`/`y` for the
  first 100 samples.
- The dataset loop only processes 100 of the 10,000 available images so far.
- `requirements.txt` is a raw environment freeze (including Windows-specific
  conda build paths) rather than a minimal, cross-platform dependency list.

## Roadmap (not yet implemented)

- Build a PyTorch `Dataset`/`DataLoader` over the full 10,000-image set.
- Define and train a classifier/regressor mapping the flattened QR bitmap to
  its encoded number.
- Add an evaluation step and accuracy/error metrics.
