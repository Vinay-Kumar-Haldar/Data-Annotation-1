# Annotation Guidelines

**Project:** Data Annotation (CVAT)
**Labels:** `person`, `building`, `sheep`
**Version:** 0.1 (draft) · 2026-10-01
**Companion file:** `quality_check.xlsx`

> Items marked **[Proposed]** are not visible in the supplied screenshots and need lead sign-off. They are also tracked in the Error Log sheet of `quality_check.xlsx`.

---

## 1. Purpose and scope

These rules tell annotators how to label images in CVAT so that every image is labelled the same way, and tell reviewers what to check.

Only the three labels in `label.txt` are annotated. Anything else in an image is left unlabelled, however prominent it is.

| In scope | Out of scope (do not annotate) |
|---|---|
| person, building, sheep | cars, zebras, trees, sky, water reflections, text, and any other object |

---

## 2. Dataset overview

| # | File | Contents | Label(s) | Shape | Expected objects |
|---|---|---|---|---|---|
| 1 | `buiding.avif` | Stepped residential tower block | building | Polygon | 1 |
| 2 | `car.avif` | Parked car at dusk | none | none | 0 |
| 3 | `person.avif` | Woman, long hair, white t-shirt | person | Points (5) | 1 |
| 4 | `person2.avif` | Woman, hair in a bun, white t-shirt | person | Points (5) | 1 |
| 5 | `sheeps.avif` | Sheep grazing on a grass slope | sheep | Rectangle | 6 |
| 6 | `walking.avif` | Group walking along a brick street | person | Points (5) **[Proposed]** | 5 |
| 7 | `zebras.avif` | Zebras at a waterhole | none | none | 0 |

Notes:
- `buiding.avif` is misspelt in the filename. Do not rename it; the CVAT task already uses this name.
- Expected counts are a visual count made when this guide was drafted. If you disagree with a count, log it in the Error Log instead of silently changing it.

---

## 3. Label definitions

### 3.1 `building`
A man-made structure with walls and a roof, meant for people to live or work in.
- **Include:** the full visible structure, from the base up to the highest point.
- **Exclude:** sky, trees, cranes, and other buildings in the background that are not the main subject.
- **Stepped or joined blocks:** a single connected structure whose sections step up in height is **one** building and gets **one** polygon (this is how `buiding.avif` is annotated in the screenshot).

### 3.2 `sheep`
Each individual sheep, adult or lamb.
- One rectangle per animal.
- Count every sheep that is clearly recognisable, including small distant ones (the two small sheep at the top right of `sheeps.avif` are annotated).

### 3.3 `person`
A human being. In the current setup, a person is marked with 5 facial landmark points.
- Real people only. Do not label statues, posters, or reflections.

---

## 4. Shape rules

The screenshots show a different CVAT shape for each label. Use exactly these:

| Label | CVAT shape | Objects panel shows |
|---|---|---|
| building | Polygon | `POLYGON SHAPE` |
| sheep | Rectangle | `RECTANGLE SHAPE` |
| person | Points | `POINTS SHAPE` |

Never mix shapes for the same label within one image.

### 4.1 Rectangle (sheep)
1. Choose the rectangle tool, set the label to `sheep`, and draw.
2. Place each edge on the outermost visible part of the animal (head, tail, hooves, wool).
3. Keep margins to a few pixels. No part of the animal may fall outside the box.
4. Do not shrink a box to avoid overlap. Overlapping boxes are fine when sheep overlap.
5. Include the body only as seen; do not extend the box to cover a shadow.

### 4.2 Polygon (building)
1. Choose the polygon tool, set the label to `building`, and click around the outline.
2. Place a vertex at every corner and every step in the roofline. Add extra vertices along long diagonal or curved edges.
3. Follow the building edge, not the sky or trees next to it.
4. Edges must not cross each other.
5. If the building is cut off by the image border, run the polygon along the image border.
6. Close the shape and check that it is selected under the right label before saving.

### 4.3 Points (person)
Each person is **one Points object with 5 points**, placed in this order **[Proposed order]**:

| Order | Landmark | Where to click |
|---|---|---|
| 1 | Left eye (viewer's left) | Centre of the pupil/iris |
| 2 | Right eye (viewer's right) | Centre of the pupil/iris |
| 3 | Nose | Tip of the nose |
| 4 | Mouth, left corner | Where the lips meet at viewer's left |
| 5 | Mouth, right corner | Where the lips meet at viewer's right |

- Zoom in before placing points.
- Do not add extra points (chin, ears, hairline) unless the lead extends the scheme.
- All 5 points belong to one object. Never split a face across several objects.

---

## 5. Edge cases

| Situation | Rule |
|---|---|
| Object partly hidden by another | Annotate the visible part if the object is still recognisable. |
| Object cut off by the image edge | Annotate what is visible, up to the edge. |
| Two sheep overlapping | Two separate rectangles. |
| Very small or distant object | Annotate if you can tell what it is at 200% zoom; otherwise skip and note it. |
| Reflection (water, glass) | Do not annotate. |
| Out-of-scope object (car, zebra) | Do not annotate. Items must read 0. |
| Group of people (`walking.avif`) | **[Proposed]** Annotate each person whose eyes, nose, and mouth can be located. For anyone who cannot be marked reliably, flag it in the Error Log rather than guessing. Awaiting lead decision (E-002). |
| Face turned away or hidden | Do not guess landmark positions. Flag it. |
| Unsure about a label | Do not invent a new label. Ask the lead and log it. |

---

## 6. Per-image instructions

**`buiding.avif`**: One polygon labelled `building` around the whole stepped tower block. Place vertices at each step. Reference screenshot shows 1 object (ID 1).

**`car.avif`**: No annotation. Confirm Items = 0.

**`person.avif`**: One Points object labelled `person` with 5 face landmarks. Reference screenshot shows this layout. Note the object ID shown is 2 while Items = 1; confirm nothing was deleted by mistake (E-001).

**`person2.avif`**: One Points object labelled `person`, 5 landmarks, same scheme as `person.avif`. No reference screenshot was supplied for this image.

**`sheeps.avif`**: Six rectangles labelled `sheep`: two small ones at the top right, two in the middle, one at the left, one in the foreground. Reference screenshot shows Items = 6.

**`walking.avif`**: Five people. **[Proposed]** one 5-point `person` object each where the face is readable. Awaiting lead decision (E-002).

**`zebras.avif`**: No annotation. Four zebras and their reflections are all out of scope. Confirm Items = 0.

---

## 7. Annotator workflow

1. Open the task in CVAT and go to the first image.
2. Read the per-image instructions in section 6.
3. Draw each object using the correct shape and label.
4. Check the **Objects** panel: the item count matches the expected count, and every item shows the right label and shape type.
5. Check that nothing is locked or hidden by accident.
6. Press **Save** (or Ctrl+S) before moving to the next image.
7. Record the object count in `quality_check.xlsx` → *Image QC* → column G.

---

## 8. Quality check

Reviewers use `quality_check.xlsx`.

- **Image QC sheet:** one row per image. Fill the object count and the four Yes/No/N/A checks. The *Count check* and *QC result* columns calculate themselves.
- **QC Checklist sheet:** the 16 criteria (QC-01 to QC-16) behind those checks.
- **Error Log sheet:** one row per defect, with category, severity, and status.
- **Summary sheet:** pass rate, review progress, and open errors by severity.

### 8.1 Severity

| Severity | Meaning | Example |
|---|---|---|
| Critical | Wrong or missing data; the image cannot be used | Wrong label; sheep missing |
| Major | Wrong shape or poor geometry; needs rework | Polygon used for sheep; box cuts off a sheep |
| Minor | Small cosmetic issue | Box margin slightly loose |
| Info | Observation or question; no fix needed yet | Gap in object IDs |

### 8.2 Pass criteria **[Proposed]**
An image passes only when:
- the object count equals the expected count,
- all labels and shape types are correct,
- boundaries and points are acceptable, and
- there are no extra or out-of-scope objects.

Any Critical or Major error means **Fail**. The annotator fixes it, the status changes to *Fixed*, and the reviewer sets it to *Verified*.

---

## 9. Open questions

| ID | Question | Status |
|---|---|---|
| E-002 | Points or rectangles for groups of people such as `walking.avif`? | Open |
| E-003 | Should car and zebra stay unannotated, or be added to the label set? | Open |
| E-001 | Why is the object ID 2 on `person.avif` when Items = 1? | Open |
| E-004 | Rename `label.txt.txt` to `label.txt`? | Open |

---

## 10. Revision history

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-01 | First draft from the supplied files, 7 images and 3 CVAT screenshots. |
