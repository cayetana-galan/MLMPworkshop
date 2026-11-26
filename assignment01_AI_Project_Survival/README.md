**Project**

Repository: https://github.com/DepthAnything/Depth-Anything-V2

Inference task: Monocular depth estimation

**Repo map**
Environment: requirements.txt

Entry point: run.py

Model:  Depth Anything V2 – ViT-Small (vits)

Input: images/whitehorse.jpeg

Output: vis_depth/whitehorse.png

**Environment setup**
```text
git clone https://github.com/DepthAnything/Depth-Anything-V2
cd Depth-Anything-V2
uv python install 3.10
uv venv --python 3.10
source .venv/bin/activate
uv pip install -r requirements.txt
```

**Model Setup**

Create the directory:
```text
mkdir -p checkpoints
```
```text
wget -O checkpoints/depth_anything_v2_vits.pth \
  https://huggingface.co/depth-anything/Depth-Anything-V2-Small/resolve/main/depth_anything_v2_vits.pth
```

**Input setup**

Create the input directory:
```text
mkdir -p images
```
Download the image (JPEG) and set a name, example: whitehorse.jpeg

To save the image on the Image folder you just created, copy this on the terminal:
```text
pwd
ls ~/Downloads
cp ~/Downloads/whitehorse.jpeg images/
```
check whether it exist:
```text
ls -lh images/
```
**Inference** 
```text

python run.py \
  --encoder vits \
  --img-path images/whitehorse.jpeg \
  --outdir vis_depth \
  --input-size 518 \
  --pred-only \
  --grayscale
```

**Type the Output**

To check the output:

```text
ls -lh vis_depth/
```
You should see the image.png, example: whitehorse.png

Open the output image:
```text
xdg-open vis_depth/whitehorse.png
```
**One real failure**

Category: Version conflict

Root cause: the full declared dependency set is not resolvable for Python 3.8 because the optional export extra includes keras>=3.5.0,<=3.12.0, which requires Python >=3.9 / >=3.10 depending on version. So the effective minimum for the full dependency set is Python 3.9+, while the repo metadata still says >=3.8.

Minimal fix: change to Python 3.10

**AI agent check**

Which AI coding agent did you use?: copilot

What did it change?: only venv python version

How did you verify the change?: I asked it to do this

**One real failure**

Category: Output path

Root cause: if the input is .jpg and --outdir is the same folder as the image, the result becomes .png in that same folder, not a failure. The issue is that the user expected the original file to be replaced or the app to fail when output is written alongside the input.

Minimal fix: Save results to a separate output folder, such as depth_vis, and keep the original image in place

**AI agent check**

Which AI coding agent did you use?: copilot

What did it change?: Confirmed the project behavior by reading run.py:39-73 and explained that the script saves a new PNG using the same file stem in --outdir, rather than modifying the original source file.

How did you verify the change?: I asked it to do this

