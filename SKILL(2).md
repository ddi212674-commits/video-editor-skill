---
description: Creates short vertical drama/story recap videos matching
  the supplied reference style, including fast scene cuts, a persistent
  dark-red lower title panel, a small top-right watermark, and a thin
  branded footer. Use when the user asks to turn supplied footage,
  images, or a story into a similar short-form video.
name: short-drama-recap-editor
---

# Short Drama Recap Video Skill

## Purpose

Create short-form story videos inspired by the supplied reference. The
reference uses a 720×900 (4:5) frame, approximately 34 seconds long,
with rapidly changing drama/action shots above a fixed dark-red
information panel. Do not assume the reference is 9:16: match the
requested platform format. If the user does not specify, ask whether
they want 4:5 like the reference or 9:16 for Shorts/Reels.

The goal is to reproduce the **editing language and layout**, not copy
another creator's exact footage, logo, watermark, wording, or identity.

## Visual style observed in the reference

-   Canvas: 720×900 pixels, 4:5 aspect ratio.
-   Approximate duration: 34 seconds.
-   Upper visual area: roughly the top 70--72% of the frame.
-   Lower title panel: roughly the bottom 28--30%, solid deep
    red/maroon.
-   Main title: white, bold, left-aligned, usually two or three short
    lines, positioned near the top of the red panel.
-   Footer: a very thin white horizontal rule near the bottom, with a
    small channel/series label aligned toward the right.
-   Watermark: small, semi-transparent white text/logo near the
    upper-right corner. Use only the user's own supplied brand mark or a
    neutral placeholder.
-   Editing: a sequence of short shots, generally around 1--4 seconds
    each, switching between wide shots, close-ups, reactions, and
    important object/action details.
-   Narrative: establish the odd or surprising situation, show the
    characters' reactions, reveal the key action/object, then finish on
    the consequence or payoff.
-   Keep the title panel and footer consistent from beginning to end;
    only the footage in the upper area changes.

## Required workflow

### 1. Inspect the inputs

1.  List and inspect all supplied video, image, audio, and text assets.
2.  Confirm the intended output platform and aspect ratio. If
    unspecified, ask one concise question: "Mau mengikuti rasio
    referensi 4:5 atau dibuat 9:16 untuk Shorts?"
3.  Identify the story's central event, characters, conflict, unusual
    detail, and payoff.
4.  Use only footage and assets the user provides or has permission to
    use. Do not source pirated drama clips or remove another creator's
    watermark.
5.  If there is no usable footage, offer a storyboard and an
    asset-generation plan rather than pretending a finished video has
    been rendered.

### 2. Build the story

Create a compact narrative with this structure:

-   **0--3 seconds --- Hook:** the strangest, most emotional, or most
    curiosity-provoking moment. Avoid a long introduction.
-   **3--10 seconds --- Setup:** establish who/what is involved and what
    is happening.
-   **10--22 seconds --- Escalation:** show reactions, attempts,
    complications, and meaningful details.
-   **22--30 seconds --- Reveal:** show the central action, object, or
    twist clearly.
-   **Final seconds --- Payoff:** show the consequence or a satisfying
    final beat. Do not add a fake twist.

Adjust timings to the actual duration and available footage. Do not
force every story into these exact timestamps.

### 3. Prepare the shot plan

Create a shot list before editing. For every shot include:

-   start and end time;
-   source file and source in/out points;
-   what the viewer sees;
-   the narrative purpose;
-   any crop, zoom, or reframing needed;
-   optional sound effect or voice-over cue.

Prefer meaningful visual changes over random rapid cuts. Use close-ups
for emotion and detail, wider shots to explain spatial context, and
reaction shots to make the story understandable.

### 4. Compose the frame

For the reference-like 4:5 layout:

-   Output: 720×900 or a higher-resolution 4:5 equivalent.
-   Reserve approximately 70--72% of the height for footage.
-   Reserve approximately 28--30% for the deep-red lower panel.
-   Fit footage with a consistent crop. Keep faces and important actions
    inside the visible upper area.
-   Use a deep maroon/red panel (suggested starting point: `#8F0000`;
    adjust to match the user's brand).
-   Main title: white, bold sans-serif, left aligned, with comfortable
    margins.
-   Keep the title short enough to read on a phone. Prefer 2--3 lines
    and avoid covering the footage.
-   Add a thin white footer rule and a small series/channel label near
    the bottom.
-   Place a small watermark at the upper-right only if the user supplies
    their own logo/wordmark. Keep it subtle and clear of faces.
-   Do not reproduce the reference creator's watermark, logo, exact
    title, or unique branding.

For 9:16 output, preserve the same design language but rebuild the
composition for 1080×1920 (or 720×1280): footage occupies the upper area
and the lower title panel remains readable without becoming excessively
tall. Do not simply stretch a 4:5 frame.

### 5. Text and audio

-   Write one strong headline for the persistent lower panel.
-   Use natural, concise language. Do not invent facts or imply events
    that are not shown or supported by the supplied source.
-   If voice-over is requested, write a conversational script that
    follows the visual sequence. Keep sentences short and synchronized
    to the shots.
-   Add music and sound effects only when the user has supplied them or
    they are properly licensed/available for use.
-   Keep dialogue and narration intelligible. Duck background music
    under speech.
-   Use captions/subtitles only if requested or useful; ensure they do
    not collide with the fixed title panel.

### 6. Render and quality-check

Use FFmpeg when available. Before rendering, confirm:

-   all source paths exist;
-   the selected clips cover the full timeline;
-   no black gaps or unintended frozen frames;
-   the title panel remains fixed and consistent;
-   text is legible at phone size;
-   the watermark is the user's own asset;
-   audio does not clip;
-   output duration and aspect ratio match the request.

After rendering, inspect the exported video from beginning, middle, and
end. If possible, inspect a contact sheet of frames. Fix cropping, text
overflow, timing, or audio problems before delivery.

## FFmpeg implementation guidance

Use a reproducible FFmpeg pipeline. First normalize each selected clip
to the target dimensions, frame rate, pixel format, and audio format.
Then concatenate the normalized clips, add the lower panel and text, add
the user's watermark if supplied, and encode the final file.

Example layout filter for a 720×900 reference-style canvas. This is a
template; adjust fonts, paths, text, and timing to the actual project:

``` bash
ffmpeg -i input.mp4 \
  -vf "scale=720:648:force_original_aspect_ratio=increase,crop=720:648, pad=720:900:0:0:black, drawbox=x=0:y=648:w=720:h=252:color=#8F0000:t=fill" \
  -c:v libx264 -pix_fmt yuv420p -c:a aac output_base.mp4
```

For text overlays, use `drawtext` with a font file that exists on the
system. Escape punctuation and special characters correctly. Prefer
loading long text from a text file with `textfile=` rather than
embedding complicated text in the filter string. Verify font
availability before rendering.

For 9:16, use a 1080×1920 canvas and calculate the footage/panel heights
proportionally. Do not stretch the source image; crop or pad
intentionally.

## Deliverables

Unless the user asks for something else, provide:

1.  A concise story angle and headline.
2.  A timed shot list.
3.  The lower-panel title text and optional voice-over.
4.  The edit/render script or project instructions.
5.  The exported video, if the runtime has the required assets and
    tools.

If rendering is not possible because footage, fonts, audio, or
dependencies are missing, clearly state what is missing and still
provide the usable shot plan and editing instructions.

## Important constraints

-   Never fabricate a scene, quote, reaction, product claim, or ending.
-   Never claim the video has been rendered unless an output file was
    actually created and checked.
-   Never remove or obscure third-party watermarks or copyright notices.
-   Do not copy another channel's branding. Use the user's own brand
    assets or neutral placeholders.
-   Keep the visual layout consistent, but adapt story pacing to the
    material rather than applying arbitrary cuts.
