# Unity Quick Reference (7/30/2026)

Animation, gamification, asset import, and Git survival.

**Project constants:** Unity 6.5 · URP · Timeline · Cinemachine · new Input System · StarterAssets third-person controller · **60 fps (not 30)**

---

## 1. Viewport Navigation

| Key | Does |
|---|---|
| `F` | Focus camera on selected object (fixes the zoom/pivot lag) |
| `Alt` + Left Click | Orbit camera around the focused object |
| `Alt` + Right Click | Smooth zoom (often more precise than the scroll wheel) |
| Middle Click (hold) | Pan the scene view |
| `Shift` + `F` | Lock view to object (camera follows it if it moves) |

---

## 2. Animation & Timeline

| Shortcut / Path | Opens |
|---|---|
| `Ctrl` + `6` | Animation window — for animating individual objects |
| `Window --> Sequencing` | Timeline window — for multi-object cutscenes |

### Animation window gotchas

- [ ] Select the correct GameObject in the Hierarchy **before** editing keys.
- [ ] Hit the red **Record** button to auto-keyframe changes made in the Inspector.
- [ ] **Turn recording off when done**, or you will accidentally animate your layout edits.
- [ ] The blue line in the timeline is your current frame preview.

### Timeline (cutscene) quick steps

1. Create an empty GameObject named `Cutscene_Manager`.
2. Add a **Timeline** component to it.
3. Drag characters or cameras into the track list to create:
   - **Animation Tracks** — trigger character animations.
   - **Activation Tracks** — turn objects/UI text on or off at specific seconds.
   - **Audio Tracks** — voiceover narration or background cues.

> **Emitters are fixed points in time.** As the Doctor would say. Move things around in Timeline all you like, but **realign the emitter when you're done** — if you move the track an emitter emits from, the emitter does *not* follow it.

### Video Player settings for Timeline-driven video

| Setting | What it actually does |
|---|---|
| **Play On Awake** | Misleadingly named. Fires on `OnEnable` — the moment the GameObject becomes active — **not** at scene load. Must be **ON** for a video driven by an Activation Track, or the track enables the object and nothing plays. |
| **Wait For First Frame** | A loading buffer: don't start video or audio until the movie file's first frame is fully loaded onto the Render Texture. Prevents the brief black flash or lag spike at the moment the object turns on. |

Unlike MonoBehaviour `Awake()`, which runs once, `Play On Awake` **re-fires every time the object is re-enabled** — so a re-shown video replays from the start.

---

## 3. UI, Canvas & HUD Bars

### Canvas rules

- [ ] **Canvas Scaler:** UI Scale Mode --> *Scale With Screen Size*, reference resolution `1920 x 1080`.
- [ ] **Anchors:** set the anchor (crosshair icon in RectTransform) for *every* UI element. Unanchored elements drift off-screen or overlap on different monitors.
  - Center elements (crosshair, popup box) --> anchor to **Center**.
  - Top/side elements (threat bar, progress bar, exit button) --> anchor to **Top-Stretch** or **Top-Right**.
- [ ] **Pivot points:** default is center `(0.5, 0.5)`. For a progress bar or sliding panel, set **Pivot X = 0** so it scales cleanly from the left instead of shrinking toward the middle.
- [ ] **Game window aspect ratio:** closing the Game window reverts it to *Free Aspect*. Reopen it and reset the aspect ratio before every playthrough (probably `1920x1080`).

### Building a left-to-right progress / threat bar

Use `UI --> Slider`, then:

1. Delete the **Handle** object.
2. Uncheck **Interactable**.
3. Drive the `Value` field (0 to 1) from a script.

Generated hierarchy:

```text
Threat_Bar               (the main Slider object)
├─ Background            (the empty/dark bar behind the fill)
├─ Fill Area             (handles left-to-right calculation)
│  └─ Fill               (the colored image that grows/shrinks)
└─ Handle Slide Area     (the mouse knob)
   └─ Handle             ← delete this
```

Put emergency overlays on their own **UI Canvas** with a high **Sort Order** (e.g. `10`) so critical elements always render on top of standard game UI.

> **TMP emoji trap:** use colored `Image` swatches instead of emoji glyphs. Emoji need TMP atlas configuration that is easy to miss and renders as boxes when you forget.

---

## 4. Interaction & Events

- **UI Buttons:** use the `On Click ()` event box in the Inspector to trigger scripts or toggle panels.
- **Walk-into-a-zone triggers:**
  1. Add a Collider to the object (e.g. Box Collider).
  2. Check `[X] Is Trigger`.
  3. Make sure the learner's avatar has a Rigidbody.
- **Component-typed slots:** a slot that asks for a component accepts the **GameObject that hosts** that component. Drag the parent object in, not the component.
- **Runtime animation wiring is per-instance and does not propagate.** What makes an object's animation *play* — the Animator state, the Timeline track, or the interact-script that triggers the clip — is component wiring on that specific instance. So "replace one FBX and they all animate" is true for the **visual capability** and false for the **interactive trigger**. You still wire the open-trigger on every instance you want functional. See §6 for what the FBX swap does and doesn't carry.
- **Disabling Jump:** comment out `JumpInput(value.isPressed)` in `StarterAssetsInputs.cs`. Fragile — a package reimport undoes it.

---

## 5. Starter Scripts (C#)

Create via `Right-click --> Create --> C# Script`. Names must match exactly.

### `TrainingEventManager.cs`

Attach to an empty GameObject named `Training_Manager` to handle UI buttons or sequence completions.

```csharp
using UnityEngine;
using UnityEngine.Events;

public class TrainingEventManager : MonoBehaviour
{
    // Creates a drag-and-drop action box in the Inspector
    public UnityEvent onTrainingEventTriggered;

    // Call from a UI Button's On Click() event, or from another script
    public void TriggerTrainingEvent()
    {
        Debug.Log("Training Event Activated!");
        if (onTrainingEventTriggered != null)
        {
            onTrainingEventTriggered.Invoke();
        }
    }
}
```

### `ProximityTrigger.cs`

Attach directly to a 3D hazard zone or doorway to fire a cutscene or alert when stepped in.

```csharp
using UnityEngine;
using UnityEngine.Events;

public class ProximityTrigger : MonoBehaviour
{
    public UnityEvent onPlayerEnterZone;

    // Fires when a GameObject with a Rigidbody enters this Trigger Collider
    private void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Player"))
        {
            Debug.Log("Player entered the training zone.");
            onPlayerEnterZone.Invoke();
        }
    }
}
```

---

## 6. Asset Import Troubleshooting

### Scale

**Root cause:** Maya works in **centimeters**, Unity in **meters**. Anything from Shian arrives with a unit mismatch. The direction of the error depends on the file format, not on who made it:

| Format | Arrives | Corrected with Scale Factor |
|---|---|---|
| `.obj` | **Huge** | `0.01` — forklift, observed 5/11 |
| `.fbx` | **Tiny** | `100` — hand cart |

Blender-native FBX has been landing at `1.0` in this project — separate pipeline, don't apply the Maya numbers to it.

#### The checkbox: Convert Units

Lives on the **FBX importer --> Model tab**, directly beneath the read-only **File Scale** readout. Older Unity versions labeled it *Convert File Units*. It reads the unit scale declared in the FBX header and converts to Unity meters, so instances land at Transform scale `1`.

**It only exists for FBX.** The OBJ format declares no units anywhere, so there's nothing for Convert Units to read — `Scale Factor` is the only lever on an OBJ.

#### Fix at the importer, not the Transform

Select the asset in the Project view and set `Scale Factor` / `Convert Units` there — not by typing `0.01` into a scene instance's Transform. Importer-level corrections apply to every future instance and keep Transform scale at `1`, which matters for physics, colliders, and animated children.

#### Maya-side, for Shian

Maya can't export in meters without baking a scale onto the object, because it's centimeters internally even when the display says meters. A clean scale-`1` import means modeling in cm as though it were meters, then importing with **Convert Units unchecked**. Not fixable from inside Unity.

### Wrong-color diagnosis

> **BLACK = stale ambient (regenerate). PINK = wrong pipeline (upgrade or revert).**

#### Black shadowed surfaces after a lighting change

Especially after deleting lights.

- **Cause:** Unity precomputes and caches the scene's ambient/environment lighting. Change what emits light — deleting bundled FBX lights, moving lights, swapping the skybox — and the cached data no longer matches the scene, so shadowed surfaces render pure black until it's recomputed. With `Bake On Scene Load = Never` (`Window --> Rendering --> Lighting --> Environment --> Other Settings`), Unity will **not** recompute automatically. That's manual on purpose.
- **Fix:** `Window --> Rendering --> Lighting --> Environment --> Generate Lighting` — the button at the bottom. Recomputes ambient data; black surfaces return to correct lighting. Fast: a recent bake ran ~3 seconds.
- **Not the cause:** this is not the machine throttling or "dropping" complex lighting under load. Unity does not silently stop rendering shadows to save compute. It's stale cached data, full stop, and Generate Lighting fixes it regardless of how busy the machine is.

#### The Pink Shader Plague — model imports bright magenta/neon pink

- **Cause:** the materials aren't compatible with the current Render Pipeline. A material/URP mismatch, **not** a lighting bake.
- **Fix:** select the material --> `Edit --> Render Pipeline --> Upgrade selected materials` (or revert via Git).
- **A different pink:** if it appears right after **Extract Materials** on an unpacked prefab, the upgrade menu won't help. Revert via Git instead.

### Other material traps

- **Maya OpenPBR --> transparent ghost:** OpenPBR imports as `Surface Type: Transparent`. Fix: assign opaque replacement materials.
- **Shared material slots:** if multiple parts share one `Element` slot, you can't recolor a part in isolation — split the mesh in Blender first.
- **Bundled lights and cameras:** imported FBXs often ship them. Delete them after import; they overflow the shadow atlas and invalidate ambient lighting — which then triggers the black-surface problem above.

### Swapping a placeholder FBX

Replacing an FBX **in Windows Explorer** (same filename) preserves the Unity `.meta` GUID — so Interactable components, colliders, and scene references survive the swap without re-wiring. This is the one sanctioned exception to the Golden Rule in §8.

**What the swap carries vs. what it doesn't:**

| Carries over | Does not carry over |
|---|---|
| GUID, and every scene reference pointing at it | Per-instance runtime wiring (§4) |
| Interactable components and colliders already on instances | The Animator state / Timeline track / interact-script that *triggers* a clip |
| The animation clips themselves — the visual capability | Which instances are actually functional |

---

## 7. Play Mode & Console

- **Play Mode Trap:** Inspector changes made *while the game is running* are lost the instant you hit Stop.
  - *Fix:* set a distinct Playmode tint — `Edit --> Preferences --> Colors --> Playmode tint` — so you always know.
- **Console errors stop execution:** a red line in the Console (`Ctrl` + `Shift` + `C`) means Play mode will freeze or fail to compile. Fix red immediately; ignore yellow.

---

## 8. Git Harmony (Unity-specific)

Full Git command reference lives in `_GitQuickReference.md`. These are the Unity-side items only.

Before `git add .`:

- [ ] Save all scenes (`Ctrl` + `S`) **and** the project (`File --> Save Project`).
- [ ] Close Unity before major reverts or branch switches (prevents file locking).
- [ ] `Project Settings --> Editor --> Version Control` = **Visible Meta Files**.
- [ ] `Project Settings --> Editor --> Asset Serialization` = **Force Text**.

**Note:** Be careful with GitHub Large File Storage on your *personal* account. It is very limited in storage and in bandwidth, with a monthly quota. Going over this can get very expensive, very quickly. More detail can be found in the Git LFS quota/billing note, which lives in `_GitQuickReference.md` §7.

> **The Golden Rule of Unity Git:** never delete, move, or rename a file outside the Unity Project window.
>
> Unity keeps a hidden `.meta` file for every asset. Move `Character.fbx` in File Explorer and its `.meta` stays behind — breaking every texture and animation link tied to that character. Always rename and move inside Unity's **Project** tab.
>
> *Exception:* overwriting a file in place with an identical filename (see §6, placeholder FBX swap).

---

## 9. Hard-Won Miscellany

- **License revoked — twice.** A Unity Industry license allows anything a Pro license does *and more*. Stop second-guessing it.
- **Timeline GUID verification:** when disconnecting a scene from a shared Timeline asset, the authoritative done-signal is `m_PlayableAsset` flipping to the new GUID — **not** a zero-match grep. Unity leaves orphaned `m_SceneBindings` entries pointing at old track GUIDs; they're inert and self-clear during scene authoring.
