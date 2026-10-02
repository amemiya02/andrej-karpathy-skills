# Output Formats Reference

Detail for Principle 5, Explain for Understanding. Read the section for the rung you are on.

## 1. Controlled Prose: ASD-STE100 Quick Reference

ASD-STE100 (Simplified Technical English) is a controlled language from aerospace maintenance documentation. Part 1 has writing rules. Part 2 is a dictionary of about 900 approved words: one part of speech and one meaning per word. Source: [asd-ste100.org](https://www.asd-ste100.org).

Aim for about 80% of the full spec. Follow a rule when it makes the text clearer. Break it when a precise technical term or code identifier needs it.

### Limits

| Item | Maximum |
|------|---------|
| Procedural sentence (an instruction) | 20 words |
| Descriptive sentence | 25 words |
| Paragraph | 6 sentences, one topic |
| Noun cluster | 3 words ("cache eviction policy" - fine; "user session cache eviction policy" - split it) |
| Instructions per sentence | 1 (except simultaneous actions) |

### Verb forms

| Form | Example | Use |
|------|---------|-----|
| Imperative | Close the valve. | Yes |
| Simple present | The valve closes. | Yes |
| Simple past | The valve closed. | Yes |
| Simple future | The valve will close. | Yes |
| Infinitive | Turn the knob to close it. | Yes |
| Past participle as adjective | The closed valve | Yes |
| Progressive (-ing) | The valve is closing. | No |
| Perfect | The valve has closed. | No |
| Passive, in procedures | The valve must be closed. | No |

Descriptive text can use the passive when the actor is unknown or irrelevant. Use "-ing" only inside a technical name ("landing gear", `is_loading`).

### Word swaps

| Not this | Use this |
|----------|----------|
| utilize | use |
| commence, initiate | start |
| ensure | make sure |
| prior to | before |
| subsequent to | after |
| approximately | about |
| in order to | to |
| replenish | fill |
| close (adjective) | near |
| facilitate | help, let |
| a number of | some, many (or the number) |
| in the event that | if |

One word, one meaning: if "close" is a verb, never use it to mean "near". Pick one name for each thing ("the worker", not "the worker", "the job runner", and "the daemon").

### Structure

- Put the topic in the first sentence of each paragraph.
- Use a vertical list for steps, options, and conditions with more than two parts.
- Keep articles and connectors ("the", "a", "this", "that"). Telegraph style is harder to read, not easier.
- Write a safety instruction as a command, then the risk: "Do not force-push to `main`. It deletes other commits." In STE, WARNING means a risk of injury and CAUTION means a risk of damage. In software, use WARNING for data loss or security risk and CAUTION for recoverable damage.

### Before and after

> It is imperative that the operator ensures the hydraulic reservoir is replenished prior to commencing operation.

> Make sure that the hydraulic reservoir is full before you start the operation.

> The deployment was failing because it had been configured with a stale token that was being cached by the runner.

> The deployment failed. The runner cached an old token, and the deployment used that token.

## 2. Diagrams

Pick the diagram type from the content:

| Content | Diagram |
|---------|---------|
| Steps and decisions | Flowchart |
| Messages between parts over time | Sequence diagram |
| Modes and transitions | State diagram |
| Parts and how they connect | Box-and-arrow / architecture |
| Data shapes and relations | ER or class diagram |
| A change | Before and after, side by side |

Rules:
- Render where the user reads: Mermaid in Markdown, ASCII box drawing in a terminal, SVG in a page.
- One idea per diagram. Above about 12 nodes, split it or group nodes.
- Label every node and every arrow. An unlabeled arrow hides the key fact.
- Use the real names from the code (`AuthMiddleware`, not "the auth thing").
- Add one or two sentences under the diagram that say what to notice.

## 3. HTML Pages

Use a page when the reader needs to explore: filter, sort, toggle, step through, or compare.

- One `.html` file with inline CSS and JS. CDN libraries are fine (e.g. Mermaid, Chart.js, D3), but they need a network connection.
- Embed the real data as a JSON literal in the page. Do not invent sample numbers.
- Readable on a phone and in dark mode. Plain system fonts are fine.
- No secrets, tokens, or private data in the page. No tracking scripts.
- Put the file in a temp or scratch directory, not the repo, unless asked.
- Load it in a browser (a headless browser if you have one) and check the console for errors before you hand it over. If you cannot check it, say so.
- Give the path and a three-line summary of what the page shows, so the user knows what to look for.

## 4. Explainer Videos

Use a video for a concept that unfolds over time: an algorithm, a data flow, a proof, a training loop. Plan first. A video takes longer to fix than to plan.

1. **Script and storyboard.** Write the narration as short spoken sentences (the STE rules apply). Pair each line with the visual it needs. For a video longer than about 90 seconds, show the storyboard to the user before you render.
2. **Narration.**
   - Paid: ElevenLabs or a similar TTS API. Read the key from an environment variable (e.g. `ELEVENLABS_API_KEY`). Never write the key into code, logs, or commits.
   - Free and local: Piper, Kokoro, or macOS `say -o line01.aiff "..."`.
   - Make one audio file per line, so you can time each scene to its audio.
3. **Visuals.** 3Blue1Brown style: [Manim Community](https://www.manim.community) (`manim -qh scene.py SceneName`). Alternative: an HTML canvas animation captured frame by frame. Set each scene length from the duration of its audio.
4. **Assemble.** Join the scenes and audio with ffmpeg. Add subtitles from the script if the user wants them.
5. **Verify.** Check the duration with `ffprobe`. Look at a few frames. Make sure the audio and the visuals stay in sync to the end. Report any check you could not do.

Give the user the file path, the duration, and the script. The script lets them check the content without watching.
