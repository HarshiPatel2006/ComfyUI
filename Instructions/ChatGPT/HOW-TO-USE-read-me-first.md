# Custom GPT: Music Prompt for MiniMax Music 3 - setup and use

This folder holds everything for a Custom GPT that turns an idea into the
caption and lyrics MiniMax Music 3 needs.

**Three files, two jobs:**

| file | what to do with it |
|---|---|
| `INSTRUCTIONS-paste-into-gpt.txt` | paste into the GPT's Instructions box. Do NOT upload it. |
| `KNOWLEDGE-minimax-music-3.md` | upload as the GPT's one knowledge file |
| `HOW-TO-USE-read-me-first.md` | this file, for you only. Do NOT upload it. |

(Last time a setup guide got uploaded by accident and had to be removed, so:
only the KNOWLEDGE file gets uploaded.)

## Creating the GPT, step by step

1. Go to chatgpt.com, open **GPTs** in the sidebar, press **+ Create**.
2. Switch to the **Configure** tab (skip the chat-style builder).
3. **Name:** Music Prompt for MiniMax Music 3 (or anything you like).
4. **Description:** Give it a song idea and a length, get back the caption and
   lyrics for MiniMax Music 3, ready to paste into ComfyUI.
5. **Instructions:** open `INSTRUCTIONS-paste-into-gpt.txt`, copy all of it,
   paste it in. Check the Description box afterwards: the builder sometimes
   copies the first line of the Instructions into it, which reads oddly.
6. **Conversation starters**, written the way people actually type:
   - a 60 second pop song about summer rain
   - a 30 second rap about my cat
   - fit my own lyrics into 60 seconds
   - why did my song get cut off?
7. **Knowledge:** upload `KNOWLEDGE-minimax-music-3.md`. Nothing else.
8. **Capabilities:** turn everything OFF. It needs no web search (the knowledge
   file is the authority and the open web has almost nothing accurate about
   this model), no image generation, and Code Interpreter must be OFF, because
   with it on, people you share the GPT with can download the knowledge file.
9. **Create / Share:** "Only me" while testing. Switch to "Anyone with the
   link" if you want to put it in a video description.

## Using it day to day

1. Tell it the idea and the length: `a slow ballad about a city waking up at
   dawn, 60 seconds`. Genre, voice, instruments, language: anything you fix, it
   keeps.
2. It answers with two code blocks. Copy the **CAPTION** block into the encode
   node's caption box, the **LYRICS** block into the lyrics box.
3. Set **max_duration** on the encode node to the same length you asked for.
4. Wire the encode node's **seconds** output into the empty latent audio
   node's seconds input, so the length is set once.
5. If the render cuts off, sounds too empty, or sings something strange, tell
   the GPT what you heard in plain words. It knows the fixes: fewer sections,
   a re-roll for a long intro, moving production facts out of the lyrics.

Quick sanity tests after creating it, one minute total:
- `a 30 second song about love` should come back as ONE verse and ONE chorus,
  two lines each.
- `why did my song get cut off?` should answer from the knowledge file (line
  budgets, the intro eating time, re-roll), not generic advice.

## Where this came from, and updating it

The two spec blocks inside the knowledge file are the exact measured formulas
the **Music Prompt Pixaroma** node ships with (v1.4.117), plus the render
measurements around them: the song shape table, the five-seconds-a-line pace,
the 9 to 20 second intro. In ComfyUI the node does this whole job automatically
with a local model; the GPT is the same writer for when you are in a chat
instead of a graph.

When the node's formulas change in a future release, refresh the two spec
blocks in `KNOWLEDGE-minimax-music-3.md` from the file
`nodes/_music_prompt_formulas.py` in the plugin folder, then in the GPT editor
remove the old knowledge file and upload the new one.
