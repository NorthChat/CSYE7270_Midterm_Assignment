# No Single Tool Runs the Pipeline
### AI Tool Evaluation Demo — Hallway Hunters Asset Pipeline
**Preeta Chatterjee | CSYE 7270: Building Virtual Environments | Northeastern University**

---

## Topic Claim

After reading this piece, a practitioner will understand how to evaluate and combine AI tools across a survival horror game's asset and animation pipeline well enough to choose the right tool for each production stage, without assuming any single AI tool handles all creative decisions equally well — and without burning hours on tool-category mismatches that no amount of prompt iteration will fix.

---

## What This Repository Contains

This repository is the full submission for the CSYE 7270 Take-Home Midterm. It documents five real AI tool evaluation decisions made during production of **Hallway Hunters**, a first-person survival horror game built in Unreal Engine 5.

| File | Description |
|---|---|
| `Chatterjee_Preeta_CSYE7270_Midterm_Essay.pdf` | Technical essay — five-section structure, 1,500–2,500 words |
| `Chatterjee_Preeta_CSYE7270_Midterm_Demo.ipynb` | Jupyter notebook demo with triggered failure case and Human Decision Node |
| `Chatterjee_Preeta_CSYE7270_Authors_Note.pdf` | Three-page author's note — design choices, tool usage, self-assessment |
| `Chatterjee_Preeta_CSYE7270_Midterm_Demo_ipynb_output_pdf.pdf` | PDF export of the notebook with all outputs rendered, generated via nbconvert |
| `images/` | All AI-generated outputs documented in the notebook |
| `README.md` | This file |

---

## The Five Decision Points

**Decision 1 — Floor Plan: ChatGPT over Claude**
Claude produced a dark-background PDF then an SVG that did not follow the reference layout. ChatGPT produced a hand-drawn style floor plan that accurately reflected the spatial relationships. Decision: ChatGPT for spatial documents requiring reference fidelity.

**Decision 2 — Zombie Rig Reference Sheet: Gemini over ChatGPT**
ChatGPT produced parts that were too realistic and visually inconsistent across segments. Gemini produced a flat 2D illustration with consistent linework across all body parts. Decision: Gemini for 2D character reference where stylistic consistency across parts matters more than fidelity.

**Decision 3 — Walk Cycle: Rigged animation over AI flipbook (triggered failure case)**
Both ChatGPT and Gemini failed to produce sequentially continuous walk cycle frames across multiple prompt iterations. The failure is structural — image generation models have no temporal model of motion. Decision: exit the AI pipeline for animation entirely and use hand-animated rigged sprites.

**Decision 4 — Blood Splatter Material: Human rebuild over Claude's suggestion (Human Decision Node)**
Claude's suggested material used Translucent blend mode, Unlit shading, Emissive Color connected, and a radial gradient. Every one of these was rejected. The final material uses Masked blend mode, Default Lit shading, no Emissive connection, and a noise-driven opacity chain. Decision: Claude for Niagara system structure, human judgment for material philosophy.

**Decision 5 — Animation Method Framework**
Summary conclusion: AI-generated flipbooks work for static reference images. They cannot produce walk cycles. Rigged animation preserves human control over timing, weight, and feel that a flipbook locks into fixed images.

---

## How to Run the Notebook

1. Open `Chatterjee_Preeta_CSYE7270_Midterm_Demo.ipynb` in Google Colab
2. Add the following cell at the top and run it first with whatever actual path you have for the files:
```python
from google.colab import drive
drive.mount('/content/drive')
import os
os.chdir('/content/drive/MyDrive/CSYE7270_Midterm')
```
3. Run all cells in order
4. The `images/` folder must be present in the same directory as the notebook

---

## Reproducing the Failure Case

The walk cycle failure (Decision Point 3) can be reproduced independently:

1. Send this prompt to any image generation tool (ChatGPT, Gemini, or other):
```
8-frame zombie walk cycle sprite sheet, side view facing left,
2D flat illustration style, each frame should show a very distinct walk pose,
white background, no shadows, no overlapping frames,
evenly spaced in a single horizontal row, all frames should be unique,
arms and legs should complete walk cycle in those 8 movements for flipbook animation
```
2. Save the output
3. Play the frames in sequence at 8fps
4. Observe whether the result reads as continuous motion

The notebook's `analyze_frame_similarity` function measures pixel similarity between adjacent frames. A usable walk cycle requires average similarity below 70%. All three outputs in this repo score above 70%.

---

## Human Decision Node

Located in Decision Point 4 of the notebook. The full rejection is documented in the code comments:

- **AI proposed:** Translucent blend mode, Unlit shading, Emissive Color connected, radial gradient opacity
- **Rejected because:** produces a glowing soft blob — reads as a UI element, not physical blood
- **Final decision:** Masked blend mode, Default Lit shading, no Emissive, noise-driven opacity chain
- **Source of alternative:** Surin (YouTube tutorial) + perceptual judgment running the effect in the actual level

---

## Scope: Accessibility Over Capability Ceiling

This evaluation covers tools available without a paid subscription: ChatGPT (free tier), Gemini, and Claude. Paid tools such as Midjourney are not evaluated because accessibility is part of the pipeline decision. Where paid tools might close a gap, it is a quality gap — not the structural gap in Decision Point 3. No payment tier of any current image generator solves the frame continuity problem. That failure is architectural.

---

## License

MIT License — see LICENSE file.

Original game concept, design decisions, production assets, and written analysis copyright Preeta Chatterjee 2026. AI-generated assets (floor plan, zombie rig reference, flipbook outputs) are outputs of third-party tools and are used here for educational and research documentation purposes.
