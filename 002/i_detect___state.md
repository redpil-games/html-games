# Project Blueprint: Detective Hidden Object Game

## 1. Game Overview
* **Genre:** Relaxed, Cozy Detective / Hidden Object Puzzle Game[cite: 1].
* **Core Loop:** The player is presented with a detailed scene image and a list of target items to discover[cite: 1]. Tapping/clicking an item highlights the found region on the canvas, plays visual/audio feedback, and temporarily highlights then removes the item card from the UI tray[cite: 1, 2].
* **Target Platforms:** Tablets (primary), Laptops/Desktops, and Smartphones (via responsive design)[cite: 1].
* **Tone & Experience:** Relaxed and cozy—no countdown timers, no misclick penalties, with soft visual ripples and Web Audio synthesized sound feedback for all discoveries and taps[cite: 1].
* **Orientation:** Landscape mode (16:9 or 16:10 aspect ratio base)[cite: 1].

---

## 2. Technical Stack & Layout Architecture
* **Technology:** Standalone HTML5 Canvas, CSS3, and modern vanilla JavaScript (Zero external dependencies)[cite: 1].
* **UI Layout:**
  * **Tablets & Desktops:** Split view with the main scene canvas filling the space on the left and a vertical **Item Tray** taking 140px on the right[cite: 1, 2].
  * **Smartphones:** Adaptive layout using CSS media queries (`max-width: 768px`)—the Item Tray shifts to a horizontal bottom bar (120px height) with scrollable cards[cite: 1, 2].
* **Scrollbar Spacing & Ergonomics:**
  * **Desktop:** `.items-grid` utilizes `padding-right: 12px` and `margin-right: 4px` to maintain comfortable breathing room between thumbnail cards and the vertical scrollbar track[cite: 2].
  * **Mobile:** `.items-grid` applies `padding-bottom: 8px` to ensure spacing between cards and the horizontal scrollbar[cite: 2].
* **Coordinate Mapping:** Native scene base resolution set to 1920x1080[cite: 1]. JavaScript dynamically maps client pointer coordinates (`clientX`, `clientY`) into native canvas dimensions using `getBoundingClientRect()` scaling calculations[cite: 1, 2].
* **Continuous Render Loop:** Driven by `requestAnimationFrame` to support real-time UI particle and ripple animations[cite: 1, 2].

---

## 3. Implemented Features & Systems

### 3.1. Visual Feedback & Animations
* **Touch/Click Ripples:** Canvas-rendered expanding ring feedback for every pointer interaction (differentiated visual styling for hits vs. misses)[cite: 1, 2].
* **Target Hit Ring:** Expanding animated ring with glowing shadow effects centered over found items upon successful discovery[cite: 1, 2].
* **Tray Card Discovery Sequence:** Upon a successful hit, the side tray smoothly scrolls the corresponding item thumbnail into view (`scrollIntoView`), immediately applies a glowing teal highlight (`.highlight` class) for 2 seconds, and then smoothly scales down and fades out.
* **Found State Markers:** Found items retain permanent highlight overlays on the canvas[cite: 1].

### 3.2. Web Audio Synthesizer Engine
* Pure Web Audio API sound generator requiring no external audio file assets[cite: 1].
* **Hit SFX:** Dual-tone ascending sine wave chime (1046.5 Hz -> 1318.5 Hz)[cite: 1, 2].
* **Miss SFX:** Soft downward pitch triangle wave drop (420 Hz -> 120 Hz)[cite: 1, 2].
* Automatic user-interaction unlock handler (`pointerdown`) to comply with browser audio autoplay policies[cite: 1, 2].

---

## 4. Data Structure (JSON Schema)
The game uses a data-driven structure supporting both simple bounding rectangles and complex polygon collision shapes[cite: 1, 2]:

```json
{
  "scene": {
    "id": "detective_office_01",
    "title": "The Detective's Desk",
    "width": 1920,
    "height": 1080,
    "imagePath": "images/001_001.jpg"
  },
  "items": [
    {
      "id": "001_001",
      "name": "מיצי",
      "thumbnailPath": "images/thumb_001.png",
      "found": false,
      "shape": "rect",
      "bounds": { "x": 1110, "y": 234, "width": 70, "height": 50 }
    },
    {
      "id": "001_006",
      "name": "ציפורה",
      "thumbnailPath": "images/thumb_006.png",
      "found": false,
      "shape": "polygon",
      "coordinates": [
        [245, 15],
        [275, 15],
        [275, 45],
        [245, 45]
      ]
    }
  ]
}