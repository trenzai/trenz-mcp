# AI content generation SOP

Goal: use the same `ai_generate → check_task` flow to produce product videos, product images, talking-head scripts and review analysis reports, at a quality the user can actually use. Ninety percent of the quality gap comes from the source images and the prompt, not the model, so put the effort in before submitting. Before writing, turn each selling point into a scene that can be filmed ("5,000 charge cycles" → "one charge mops a 1,300 sq ft home all week"): **the visuals prove the selling point; don't lecture through voiceover**. A selling point without a matching scene is a gap.

## Contents

- [General flow](#general-flow)
- [Images first](#principle-1-images-first)
- [Writing prompts](#principle-2-write-prompts-by-scene)
- [Choosing the type and confirming cost](#principle-3-recommend-by-scenario-the-user-decides-on-spending)
- [Acceptance checklist](#acceptance-checklist)
- [Remixing a viral video](#remixing-a-viral-video)
- [Notes](#notes)

## General flow

1. **Check the account first**: before batch generation or video generation, call `account_info` and plan around the current tool description and response. Generation runs on the user's trenz.ai plan (plan permissions apply, and calls count toward the plan's 7-day Agent allowance) and also uses trenz.ai credits. `account_info` shows the remaining 7-day allowance and the credits balance; the balance is not a price quote; how much a job deducts and what happens when credits run short come from the current response
2. **Prepare images**: the step that sets the quality floor; see "Principle 1"
3. **Write the prompt**: follow the scene templates in "Principle 2" and show the prompt to the user
4. **User confirmation**: confirm the type, key parameters and the cost with the user. State a cost only if the current tool description or response gives one; otherwise say it can't be estimated, and don't derive or invent one (see "Principle 3")
5. **Submit and poll**: ai_generate(type, params) → returns task_id → check_task until completed / failed; for video tasks wait at least 15 seconds between checks
6. **Acceptance**: check against the "Acceptance checklist". If it fails, diagnose and adjust before retrying; a retry is charged again, so don't resend it unchanged
7. **Deliver**: give the user the result URL; when a task fails, explain the error in terms the user understands

## Principle 1: images first

Image-to-video beats text-to-video. The make-or-break point of product content is **product consistency**: a product generated from text alone won't match the real item, and no matter how good the visuals are, it can't be used.

- For product videos, prefer image-to-video: pass real product photos as the reference image parameter (parameter names per type are in the ai_generate tool description; images and reference media are passed as `{ "url": ... }`)
- Image sources: product images from get_detail(products), or the user's own photos (upload local files with upload_file first; see "Notes")
- Image quality sets the floor: clear subject, no watermark, enough resolution; several angles beat a single image
- Use text-only generation only when there is no product image at all, and then lock down the product's appearance in the prompt (color / material / shape / logo position), and remind the user at delivery to check product consistency

## Principle 2: write prompts by scene

Don't translate the request literally. Turning a one-line request directly into a prompt gives cheap, empty shots. Write in English with a structured description. Skeletons for four product content scenes:

**Product showcase**
[camera move] + [product and material details] + [lighting] + [background / mood]
> Slow orbit shot of a matte black insulated tumbler on a wet stone surface, water droplets on the lid, soft morning light from the left, shallow depth of field, cinematic product photography style

**Lifestyle / in-use**
[person + action] + [the product's role in the shot] + [setting] + [mood / texture]
> A young woman in gym clothes grabs the tumbler from a car cup holder and takes a sip, golden hour sunlight, casual handheld camera feel, authentic UGC style

**Unboxing / close-up**
[hand action] + [order of detail close-ups] + [texture keywords]
> Close-up of hands lifting the tumbler out of kraft packaging, macro shot of the engraved logo, fingers running over the silicone grip, crisp focus, satisfying tactile feel

**Talking head**
Script first, then generate: split the script into a shot list by hand → one prompt per shot, generated one segment at a time. Don't put a 30-second talking-head script into a single generation.

General rules:

- One prompt describes one shot and one action; for several shots, split them into a list first and generate them one by one
- Write negative requirements as positive descriptions (not "no watermark" but "clean background"); most video models have no negative prompt input
- The product appearance words in the prompt must match the reference image; don't let text and image contradict each other
- Avoid asking for text inside the frame (packaging text, captions); generated text is very likely to be garbled and can't be fixed
- If the user writes the request in another language, translate it into an English prompt and show it together with its meaning in the user's language for confirmation

## Principle 3: recommend by scenario; the user decides on spending

Before every generation, re-read the current `ai_generate` tool contract. The skill does not keep a list of models, parameter ranges or prices:

1. Confirm the currently available types from the `type` enum and the matching `oneOf` branches; only recommend options that actually exist in the contract.
2. Based on the user's output type, reference material, duration, aspect ratio, resolution and turnaround needs, compare each branch's required fields, enums, ranges and reference limits, and give one recommendation with a reason; don't hand the user a bare list of options.
3. Prices, when they are charged, and what happens on failure come only from the current tool contract or the response.
4. Before submitting, show the final type, parameters and any cost the tool can confirm. For batch jobs (>3 tasks), first give the list and the total cost if it can be confirmed; if it can't be estimated precisely, say so and don't invent one.

When the user has already specified the type and parameters, or said "you decide", don't keep asking, but never skip confirmation before a paid submission.

## Acceptance checklist

Check after every generation; don't deliver results that fail.

Video:

- Product consistency: do appearance / color / logo match the real photos? Any deformation, clipping, extra fingers?
- Text in frame: any garbled text? (If so, it basically can't be fixed; rewrite the prompt to avoid text in frame)
- Action completion: is the action finished within the duration? (If not, the prompt packed in too much)
- Aspect ratio: does it fit the placement? (Product short videos are usually 9:16 vertical)

Images: subject consistency, edge artifacts, consistent light direction, enough empty space for copy.

Common failures → fixes:

| Symptom | Fix |
|---|---|
| Product deformed / wrong details | Use a clearer reference image of the subject; cut descriptions of anything other than the product so the model focuses on it |
| Empty, cheap-looking shot | The prompt lacks lighting and material detail; fill it in following the scene skeleton |
| Action not finished | Shorten the action description, or set a longer duration |
| People look fake, stiff expressions | Re-read the current contract and choose an available type better suited to realistic people; or add authentic UGC style / handheld camera to the prompt |

Diagnose before retrying: resending the original prompt will most likely reproduce the same problem and waste money.

## Remixing a viral video

1. Find a hit on the data side: search(videos) or get_related(products, id, "videos") to pick high-GMV videos
2. ai_generate(type="script_breakdown", params={"video_id": video id, "country_code": country}) to break down the script structure and the rhythm of selling points
3. Write a new script from the breakdown plus the user's product information (plain text generation); the user confirms it
4. Split into shots: break the confirmed script into a list of 2–4 second shots, each keeping only one action
5. Prepare product images → generate shot by shot (Principles 1 and 2) → run each shot through the acceptance checklist
6. Collect the results of all shots and deliver the URLs with any necessary quality notes per the acceptance checklist

Extra material for the script: real buyer voice from get_related(products, id, "reviews") or ai_generate(type="review_analysis") (selling points from positive reviews, pain points from negative ones) can go straight into the script; it is more credible than invented selling points.

## Notes

- Reference media parameters need a publicly accessible URL. When the user gives a local file, call upload_file, PUT the file to the returned `url`, then pass the returned `signed_url` in
- Video generation takes a few minutes; after submitting, tell the user the expected wait. When using `check_task`, wait at least 15 seconds between checks; don't go silent and don't poll rapidly
- Every submission and every retry counts toward the plan's 7-day Agent allowance again and may use credits again. Explain the reason and any cost that can be confirmed, get confirmation, and don't resend unchanged
- The prompt templates only give structure; when the output falls short, first find whether the problem is the images, too much in one shot, or the type chosen, then adjust and retry
