# Storyboard and video prompt templates

Read this file when creating images, storyboards, videos or voiceover on a canvas. Before calling any tool, read the [Trenz canvas operations contract](canvas-operations.md) and strictly follow its gates: the user's visual confirmation of the skeleton and confirmation before paid generation. These templates organize creative decisions; they are not a fixed shot count, a fixed model, or prompts to copy word for word.

## Contents

- Choose a creative route from the intent
- Assets and consistency
- Organize the content first, then write the prompt
- Prompt template library: single image, light storyboard, light video, full cinematic storyboard master, compact storyboard run template, dynamic video run template
- Judging results after generation

## Choose a creative route from the intent

First decide what the user wants viewers to see, believe or feel, then choose the content form:

| Intent | Content route |
|---|---|
| Make viewers believe the product is real | Prove it with visible appearance, one usage action, a detail or a result; prefer everyday UGC, demos or comparisons. |
| Build brand character | Build emotion with people, space, color, composition and rhythm; you can make concept images first to align the style, then make the video. |
| Tell a story or voiceover dialogue | First define the relationships between the characters and one main conflict or change, then arrange the shots. Lines should sound like what the target user would naturally say. |
| Borrow a viral structure | Extract the hook, conflict, rhythm and proof method, and rewrite them with independent characters, scenes and lines; do not copy the original people, brand, footage or exact lines. When a finer breakdown is needed, first verify the video, then organize the structure by hand from what is visible. |
| Quickly explore a visual or a single action | Make a single image or a single-shot video directly; do not create a script or storyboard just for form's sake. |

When the user already has a script, lines, CTA or reference images, first carry over the story, language, call to action and prohibitions it already defines; do not add a stronger conflict, a purchase push or a second storyline on your own. When information is missing and would materially change the result, ask only the single most important question. When the user asks to generate directly, you can complete a no-cost draft with reasonable defaults, but you must still show the final concept, prompt, reference asset roles and generation parameters, and get explicit confirmation before submitting a paid job.

## Assets and consistency

For real product, person and scene assets, answer three questions first: which appearance details are actually visible, what else the user has confirmed, and what would affect accuracy but still needs to be provided. These are the agent's internal judgments, not a form for the user to fill in.

When assets are insufficient, say directly which kind of asset is needed and which visual problem it solves: detail shots restore structure, spec pages support voiceover claims, scene photos set the usage environment. If the user does not provide them, you can still make concept images or mood pieces, but do not present AI-filled details as real product footage or confirmed specs.

Use multiple images by role: the main product/person image locks the overall identity, detail images lock the matching parts, scene images lock the space, and style images only affect aesthetics. Reference roles must be written into the prompt and implemented on the canvas with real nodes and edges; a style image must not rewrite the product's or person's identity.

## Organize the content first, then write the prompt

Whatever the deliverable, settle the following first; use what the task needs, not every item every time:

- **Core purpose**: the one benefit, emotion or turn viewers should remember.
- **Subject and scene**: who or what appears, where, and which appearance details must not drift.
- **Visible evidence**: one visible action, detail, comparison, reaction or result; do not replace visuals with slogans.
- **Visual language**: everyday phone footage, controlled ad look, first-person view, close-up, tracking shot, static composition, etc.; pick one direction that matches the purpose.
- **Sound and language**: natural voiceover, ambient sound, music or silence; handle the voiceover language separately from storyboard annotations and video instructions.
- **Continuity and prohibitions**: what the subject, wardrobe, product structure and spatial relationships must keep; which invented features, on-screen text, UI or impossible actions must not appear.

The shot count and rhythm follow the purpose: proving an action can be one continuous shot; a story can follow "hook → situation/problem → action or evidence → result/memorable moment", keeping only the beats actually needed. Each shot carries one main action viewers can see clearly; do not cram conflicting actions, camera moves and text into the same moment.

## Prompt template library

Replace the brackets or braces with the current task's information; delete paragraphs that do not apply, and never submit unreplaced placeholders. These templates are optional: no task is required to use the full master, the five-zone storyboard or a fixed 8 shots.

### Single image

```text
[Subject and reference roles]
Goal: [what viewers should feel/understand].
Visual: [subject, key action or state, scene].
Composition and visual language: [shot size, camera position, color, lighting, texture].
Keep: [appearance, proportions, materials, people or spatial relationships that must not change].
Avoid: [task-relevant errors, extra text, UI or invented features].
```

### Light storyboard

```text
Create a storyboard production board, not a poster or a text-only page.
[Reference roles and subject locks]
Theme and viewer goal: [theme / emotion / one core benefit].
Scene and visual language: [space, time, lighting, color tone, camera feel].
Shot beats: [for each needed beat, write Visual / Dialogue-Sound / Purpose].
Final video goal: [video type, aspect ratio, duration, rhythm or voiceover language].
Keep: [subject identity, product appearance, wardrobe, spatial relationships, continuity].
Avoid: [board UI or text inside the thumbnails, garbled text, irrelevant shots, invented features].
```

Use a storyboard only when continuous narrative, shot rhythm or subject locking is needed. It should make the subject, scene, shots and final video goal clear; the board's annotations, borders and cards must not carry over into the final video.

### Light video

```text
[Reference roles; if there is a dedicated storyboard, state which subjects, scenes and shot information it locks]
Create a [duration / aspect ratio / resolution] video for [audience or content type].
Subject and continuity: [what must stay the same in people, product, wardrobe, space and lighting].
BEATS
[Each beat: Visual: one main action, shot size or camera move.]
[Dialogue/Sound: exact lines, ambient sound or no voiceover.]
[Purpose: what this beat proves, advances or leaves behind.]
Language: [the actual spoken language, accent, or an "exactly as written" requirement].
Avoid: [identity drift, product deformation/duplication, invented features, key action blocked, captions, prices, links, buttons, UI].
```

A single action only needs a model that supports a single shot; a multi-shot rhythm needs a model that supports multiple shots, or split the shots into clips and combine them. Model, duration, resolution and input mode always come from the current tool schema and are never hard-coded in the template. The video duration must equal the total of the timeline the user confirmed; if the current model does not support that duration, adjust or split the script first and confirm again.

### Full cinematic storyboard master

Keep it in full; it is the source of the design rules. For actual submissions you can use the compact template in the next section instead, or delete modules that do not apply to the current task.

```text
{Project theme / video theme} storyboard

As a senior film art director, commercial short-video director and visual pitch designer, generate a high-end, multi-module "cinematic storyboard / storyboard production board" based on the reference assets I uploaded and the creative direction I give.

This is not an ordinary poster or a single subject showcase image. It is a complete professional visual pitch board, like a film project pitch board, a director's storyboard, a commercial short-video script board, or an ad creative production board. Based on the real visual information in the uploaded assets and my requirements, fill in a reasonable subject setup, scene setup, camera language, lighting, color tone, composition and storyboard content.

[INPUT]

Theme/Story: {video theme, emotional direction, story core}

Subject setup: {protagonist, product, people, props, brand objects, space or core visual object; when character background and scene information are insufficient you may fill them in reasonably, but product appearance, structure, function and specs may only use facts visible in the reference images or information the user explicitly provided}

Scene setup: {main scene, e.g. living room, bathroom, backyard, kitchen, garage, office, street, hotel room}

Video type: {type, e.g. realistic short video, cinematic ad, ASMR, unboxing, cleaning challenge, lifestyle, plot twist, tutorial demo, review, vlog, TikTok UGC}

Camera language: {camera style, e.g. phone footage, slight handheld, extreme macro, smooth push/pull, tracking, locked-off, slow motion, time-lapse, first-person view}

Lighting: {light, e.g. natural daylight, soft morning light, golden hour, warm indoor light, bathroom ceiling light, overcast diffuse light, night neon}

Core color tone: {main and supporting colors, e.g. premium gray, warm white, deep blue, clean white, everyday mixed colors, low-saturation natural colors}

Composition: {composition requirements, e.g. minimalism, real home-life feel, infographic layout, left-right split, strong before/after comparison, product centered, person in foreground}

Shot count: {number, default 8 Shots}

Output format: {aspect ratio and quality, e.g. 9:16 vertical, 4K, cinematic clarity; or 16:9 horizontal; or 1:1 square}

[CORE LAYOUT AND VISUAL REQUIREMENTS]

Use a highly structured, minimalist UI grid layout. The overall aesthetic must be premium, refined and professional, using premium gray, transparent UI elements, thin frame lines, semi-transparent info cards and clear module zones to build the frame, giving the clean look of a top design tool, a film pitch board or an ad creative board.

Board annotations default to the user's language (English unless the user writes in another language); if the user explicitly specifies another board language, follow the current request; locked dialogue always stays in its original language. The layout must be clean and tidy, infographic-like and highly readable. The image must look like a complete storyboard production board that could really be used for a pitch, not a text-only description.

Keep the subject's appearance, color, proportions, material, key structure and identifying features from the uploaded reference assets consistent. Strictly avoid AI deformation, physical flaws, structural errors, wrong materials, garbled text, floating subjects, distorted proportions or unreasonable perspective.

[BOARD STRUCTURE (GRID LAYOUT)]

Output exactly these 5 zones:

1. Top (Header)

Large centered title:
"{Project title}"

The title should feel like a commercial short-video pitch: short, memorable, summing up the video's theme, emotion and core selling point.

2. Left (Character & Subject Reference)

Show the core subject references from the uploaded assets, including the subject's appearance, key parts, material, color, proportions and identifying features.

For a person, show the styling, wardrobe, expression and identity setup.

For a product or prop, show the subject itself, key accessories, and the functional details and material features that are visible in the reference images or confirmed by the user; do not invent functions that cannot be seen or are unverified.

For a space or scene, show the spatial structure, style references, core visual elements and usage atmosphere.

The left zone should work as a "visual identity reference board" that locks subject consistency; do not generate new looks that conflict with the reference assets.

3. Center (Environment + Set Design)

Show the main shooting scene and atmosphere, including the spatial layout, prop placement, light sources, signs of everyday life, weather/time, color mood and the area the camera can shoot.

The scene must serve the video's theme, not just be a pretty background. Let the scene naturally carry the later shot actions and story progression.

The center zone needs a real set feel, a director's storyboard feel and a workable shooting-space logic.

4. Right (Storyboard & Shots)

Output exactly {shot count} storyboard thumbnails. Every shot must have a matching thumbnail, labeled "Shot 1, Shot 2, Shot 3…".

Each shot must include:
[Visual] the specific content, action, subject position, camera move, shot size and scene change.
[Dialogue/Caption/Sound] the matching line, caption, narration, sound effect or ASMR sound cue.
[Selling point/Emotion] the function, emotion, conflict, twist or memorable moment this shot conveys.

Keep the shots continuous: from the opening hook, problem setup, subject reveal, process demo, detail emphasis and emotional build, to the result and the closing memorable moment or call to action.

Shot template:

Shot 1: [Visual] {opening hook: grab attention with a strong question, strong visual, strong emotion or strong action.} / [Dialogue/Caption/Sound] {one short, memorable opening caption or sound.} / [Selling point/Emotion] {the problem or emotion this shot sets up.}

Shot 2: [Visual] {the subject appears clearly for the first time, showing key appearance or usage state.} / [Dialogue/Caption/Sound] {highlight the subject's identity or core change.} / [Selling point/Emotion] {viewers know what this video will solve.}

Shot 3: [Visual] {enter the core action or core scene, showing the before-use state, conflict or pain point.} / [Dialogue/Caption/Sound] {a short caption stating the problem.} / [Selling point/Emotion] {strengthen realism and relatability.}

Shot 4: [Visual] {show the subject taking part in solving the problem, with clear action and real details.} / [Dialogue/Caption/Sound] {highlight one core advantage.} / [Selling point/Emotion] {build credibility.}

Shot 5: [Visual] {macro or detail shot showing material, function, change in action or a key moment.} / [Dialogue/Caption/Sound] {reinforce the detail in a few words.} / [Selling point/Emotion] {let viewers see concrete evidence.}

Shot 6: [Visual] {switch to a second scene or second use, proving range of use or advancing the story.} / [Dialogue/Caption/Sound] {highlight multiple scenes, multiple uses or deeper value.} / [Selling point/Emotion] {broaden persuasion.}

Shot 7: [Visual] {result comparison, a person's reaction, a change in the environment or an emotional release.} / [Dialogue/Caption/Sound] {one summarizing line or caption.} / [Selling point/Emotion] {create a before/after or emotional payoff.}

Shot 8: [Visual] {closing freeze frame, full reveal, brand-feel image, call to action or a punchline twist.} / [Dialogue/Caption/Sound] {closing CTA, memorable line or final punchline.} / [Selling point/Emotion] {make viewers remember and act.}

5. Bottom (Key parameter cards)

Arrange these 10 items neatly in a row, each in its own info card:

Theme/Story: {fill in or summarize the core theme}

Subject setup: {fill in or summarize the subject's identity, appearance and key features}

Scene setup: {fill in or summarize the main environment}

Video type: {fill in or summarize the video type}

Camera language: {fill in or summarize the camera style}

Lighting: {fill in or summarize the lighting mood}

Core color tone: {fill in or summarize the color system}

Composition: {fill in or summarize the composition logic}

Shot count: {fill in the shot count, e.g. 8 Shots}

Output format: {fill in the output aspect ratio, resolution and style, e.g. 9:16 vertical, 4K, realistic short-video look}

[STYLE REQUIREMENTS]

The whole board must have:
- a professional cinematic storyboard feel
- a commercial ad pitch feel
- a director's storyboard feel
- a multi-module grid layout
- mixed images and text
- many image thumbnails
- a clear information hierarchy in the board language
- believable subjects and scenes
- enough detail to directly guide later video generation or shooting

[CONSISTENCY REQUIREMENTS]

The subject must stay consistent across all shots, including color, proportions, material, structure, wardrobe, accessories, hairstyle, age, spatial relationships and key identifying features.

If the reference assets lack information, only fill in non-product scenes and character backgrounds reasonably; keep unknown product information unknown, do not change the subject's nature, and do not add unverified structures, functions or complex elements unrelated to the theme.

[NEGATIVE REQUIREMENTS]

Do not generate an ordinary poster.
Do not generate a single subject image.
Do not output text only.
Do not make the board look like a slide screenshot.
Do not make the image crowded or messy.
Do not include garbled or meaningless text.
Do not include AI-looking people, plastic skin, deformed fingers, structural deformation, wrong subject proportions, wrong materials, floating objects, unreasonable perspective, over-smoothed skin, cartoon look, CGI look, heavy studio look or fake exaggerated acting.
Do not change the core subject from the uploaded reference assets.
Do not make the shots a set of unrelated pretty images; they must form a continuous video script.

[FINAL OUTPUT GOAL]

Generate one complete, clear, professional "cinematic storyboard / storyboard production board" with complete information in the board language. At a glance the board should show: who the subject is, where the scene is, what the video is about, how the shots are filmed, how the emotion builds, and what style the final output has.
```

### Compact storyboard run template

The full master is long and only constrains the design. The actual `board.prompt` can use the template below; replace all bracketed content and never submit placeholders.

```text
[DYNAMIC IMAGE MENTION PREFIX, OMIT WHEN NO REFERENCES]
Generate a horizontal 16:9, 2K cinematic storyboard board, target 2048×1152; it is a complete production board, not a poster, a single subject image, a slide screenshot or a text-only page. Use premium gray, transparent UI, thin frame lines, semi-transparent cards and a compact grid; the thumbnails are continuous, realistic cinematic frames, and all annotations sit outside the thumbnails. Board annotations are in [BOARD LANGUAGE]; locked dialogue stays in its original language.

[Reference roles] Image 1: [ROLE/SCOPE/PRIORITY]; Image 2: [ROLE/SCOPE/PRIORITY]. The main identity reference overrides detail, scale and style images; each image controls only its declared scope.
[Fact boundary] Visible facts: [VISIBLE FACTS]; user-confirmed: [CONFIRMED CLAIMS OR NONE]; unknown/do not claim: [UNKNOWN OR PROHIBITED].

[Top / Header] "[PROJECT TITLE]"
[Left / Character & Subject Reference] [CONSISTENT PEOPLE, PRODUCT/SUBJECT APPEARANCE, MATERIAL, PROPORTION, WARDROBE AND IDENTITY LOCKS].
[Center / Environment + Set Design] [EXECUTABLE SET, OBJECT POSITIONS, TIME, LIGHT, COLOR AND CAMERA SPACE].
[Right / Storyboard & Shots] Output exactly [SHOT COUNT] shots:
Shot 1 — [START]-[END]s
[Visual] [ONE CLEAR ACTION, FRAMING AND CAMERA].
[Dialogue/Caption/Sound] [EXACT DIALOGUE OR SOUND].
[Selling point/Emotion] [PURPOSE].
[REPEAT THROUGH FINAL SHOT WITH CONTINUOUS TIMECODES]

[Bottom / Info Cards]
Theme/Story: [VALUE]; Subject setup: [VALUE]; Scene setup: [VALUE]; Video type: [VALUE]; Camera language: [VALUE]; Lighting: [VALUE]; Core color tone: [VALUE]; Composition: [VALUE]; Shot count: [N Shots]; Output format: [VIDEO OUTPUT].

[Style and consistency] Professional cinematic storyboard, commercial ad pitch, director's storyboard, mixed images and text; every shot keeps the same subject appearance, color, proportions, material, structure, face, wardrobe, accessories, hairstyle, age, spatial position and key identifying features. Reasonable fill-in is limited to non-product scenes and character backgrounds.
[Prohibited] Ordinary poster, single subject image, text only, crowded or messy layout, captions/arrows/UI/prices/links inside thumbnails, garbled text, AI plastic skin, deformed fingers, subject deformation or duplication, wrong structure/material/proportions, floating objects, unreasonable perspective, over-smoothed skin, cartoon/CGI/heavy studio look, irrelevant shots, unverified product structures or functions, an extra "board output spec" block.
```

### Dynamic video run template

Video instructions default to English; dialogue keeps the exact original text in the target language. The actual `video.prompt` can use:

```text
[DYNAMIC IMAGE MENTION PREFIX; STORYBOARD IS ALWAYS LAST]
REFERENCE ROLES — Image 1: [ROLE/SCOPE]; Image 2: [ROLE/SCOPE]; final image: the dedicated storyboard locking people, faces, wardrobe, positions, set geometry, acting, continuity and exact shot order. The primary identity reference overrides detail, scale and style references. Each reference controls only its declared scope. Never copy the board’s text, grid, borders, arrows, labels, cards or UI into the video.

Create a realistic [DURATION]-second [VIDEO ASPECT RATIO], [RESOLUTION] [TARGET MARKET/GENRE] video with synchronized [SPOKEN LANGUAGE] speech and natural sound. Preserve the same people, faces, wardrobe, product/subject identity, body scale, light, set and action ownership.

TIMELINE
SHOT 1 — [START]-[END]s
Visual: [ONE CLEAR ACTION, FRAMING AND CAMERA].
Dialogue/Sound: [EXACT LINE OR SOUND].
Purpose/Emotion: [PURPOSE].
[REPEAT THROUGH SHOT N WITH CONTINUOUS TIMECODES]

FACT + ACTION LOCK — Visible facts: [VISIBLE FACTS]. User-confirmed claims: [CLAIMS OR NONE]. Unknown/prohibited claims: [UNKNOWN]. Preserve one continuous physical process; never duplicate, redesign, swap, detach, hide or invent the key subject or action.

CTA + SOUND — [PRESERVE THE SOURCE CTA OR EXPLICIT NO-CTA RULE]. Speak every required line fully and clearly. Reduce competing sound before the final spoken line. Do not display a link or UI.

Theme/Plot: [VALUE]; Subject: [VALUE]; Setting: [VALUE]; Video Type: [VALUE]; Camera: [VALUE]; Lighting: [VALUE]; Color Tone: [VALUE]; Composition: [VALUE]; Shot Count: [N]; Output: [DURATION, ASPECT RATIO, RESOLUTION, STYLE].

PROHIBITED — [ALL SOURCE-SPECIFIC NEGATIVE REQUIREMENTS]. No identity drift, wardrobe change, position swap, subject deformation or duplication, unsupported feature, material/structure change, impossible physics, hidden key action, discontinuous story, captions, subtitles, titles, arrows, price, cart, link graphic, button, watermark, added logo overlay or UI.
[EXACT LANGUAGE RULE FROM settings.language_rule]
```

## Judging results after generation

After a storyboard is generated, first check subject consistency, shot readability and text hierarchy, then hand it to the user for visual confirmation; only after the user explicitly approves it can it be used as a video reference. After a video is submitted, distinguish "submitted" from "completed"; when the user asks for a quality check, check identity drift, deformation, action completion, continuity, on-screen text and voiceover. When you find a problem, first decide whether it comes from the assets, the creative direction, the storyboard, the prompt or the model's capability, then fix that layer; any redo or variant must show the changes and cost again and get confirmation, never a blind re-roll of the same request.
