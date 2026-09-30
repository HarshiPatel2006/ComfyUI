# MiniMax Music 3: how to write its caption and lyrics

MiniMax Music 3 is a song model. In ComfyUI its text encode node takes TWO
separate texts, and they are different kinds of writing:

- **caption**: a description of the SOUND: genre, tempo, key, voice,
  instruments, how the recording feels. It is never sung.
- **lyrics**: the words a voice will sing, laid out with section tags like
  [Verse] and [Chorus]. Everything here is sung out loud.

The node also takes **max_duration**: the length of the song in seconds, from a
few seconds up to 360. It is a CEILING. The model fills it with music, can end
a little early, and cuts off anything that runs past it, usually mid word.

Everything below was measured with real renders and checked by ear, not
guessed. The two spec blocks are the exact instructions that were tested; use
them as written.

---

## THE CAPTION SPEC (use as written)

You write the STRUCTURED CAPTION that a music model reads to compose a song. Turn the idea below into that caption and write nothing else.

Write it as three short labelled parts, in this order, each on its own line.

Global Metadata: name the genre and a subgenre, a BPM as a number, a key and scale, how the feeling moves from the start of the song to the end, where someone would listen to it, and how the recording should sound.

Vocal Details: say whether the voice is male or female, what the voice sounds like, how it is performed, and whether there are harmonies or backing vocals.

Arrangement: name the instruments that carry the song and the ones that support it, how the instruments change between sections, the groove, what the bass and the drums do, and how much space the recording has.

Choose words that suit THIS idea. Where the idea already fixes something, such as a tempo or an instrument, keep it exactly and build the rest around it.

Never write any lyrics, any section tag in square brackets, or any quoted words the singer would sing: this caption describes the SOUND, and the words are handed to the model separately. Do not use markdown, headings, bullet points or asterisks. Do not introduce your answer or repeat the idea back. Start with the words Global Metadata.

---

## THE LYRICS SPEC (use as written, with the line count from the table below)

You write the LYRICS a music model will sing. Turn the idea below into a song and write nothing else.

Lay it out with a section tag on its own line before each part, choosing from [Intro] [Verse] [Pre-Chorus] [Chorus] [Post-Chorus] [Bridge] [Instrumental] [Solo] [Outro].

FIT THE WORDS TO THE LENGTH ASKED FOR. A sung line takes about three seconds, so a four line section runs about twelve. Count your lines against the time you are given and stop when you reach it, because anything past the end is simply cut off. Under forty seconds, write one verse and one chorus and nothing else. Around a minute, a verse, a chorus, a second verse and the chorus again. Around two minutes, add a bridge and a final chorus. Longer than that, add another verse or a solo. When the idea does not say how long, write the two minute shape.

A section tag can stand completely alone with nothing written under it, and that means the band plays and nobody sings there. That is how you open with music or take a break, but it still uses up time, so only do it when there is room to spare. Only write a line under a tag when there are words to be sung.

Keep the lines short enough to sing in one breath. Let the chorus repeat almost the same words each time, because that is what makes it a chorus. Write in the language the idea is written in.

Every line you write is words a voice will sing, including anything inside brackets or parentheses, so keep each line to something a singer would actually sing. The instruments, the tempo and the mood are described somewhere else and are not your job here. No markdown, no quotation marks around the lines, and no note explaining what you did. Start with a section tag.

---

## SONG SHAPES: the measured table (this wins over the spec's own length prose)

The spec above says a sung line takes about three seconds. Renders checked by
ear say it is really 4.5 to 8 seconds once the gaps between sections are
counted, and slow deliveries stretch the most. So the safe line counts are:

| length asked | shape to write | sung lines total |
|---|---|---|
| under 40 seconds | one verse and one chorus, two lines each | 4 |
| 40 to 90 seconds | two verses and two choruses, two lines each | 8 |
| 90 seconds and over | the spec's own prose shape: verse, chorus, verse, chorus; add a bridge and final chorus around two minutes; add another verse or a solo beyond that | model paced |

For a length not in the table, budget about five seconds per sung line, subtract
10 to 20 seconds for the intro the model usually plays before the first word,
and round DOWN.

Why aim short: a short lyric costs nothing, because the model keeps playing
music to the end of the requested time. A long lyric always loses its ending.
Measured: a 16 line lyric at 60 seconds got through 9 or 10 lines and cut mid
word; an 8 line lyric fits.

## Verse counts are a request, not a promise

Measured across seeds: asking for 1 verse or 2 verses comes back exactly.
Asking for 3 sometimes returns 2. Asking for 6 returned 5. Do not promise
counts above 3.

## The intro is seed luck

The same song rendered twice opened with 9 seconds of music one time and 20 the
next. No wording controls it. When an intro feels too long, re-roll the seed
rather than rewriting the words.

---

## WIRING IN COMFYUI

The chain is: text encode node, then an empty latent audio node, then the
sampler and the audio save.

- Paste the caption into the encode node's **caption** box and the lyrics into
  its **lyrics** box.
- Set **max_duration** to the length the lyrics were written for. The two must
  agree: lyrics written for 60 seconds against a 30 second ceiling get chopped.
- The encode node has a **seconds** OUTPUT: the length the model actually
  produced. Wire that output into the empty latent audio node's seconds input,
  so the length is set once and the whole chain follows it.

In ComfyUI the **Music Prompt Pixaroma** node does this whole job automatically:
one idea box, length and verse buttons, and it writes both texts with a local
model, with caption, lyrics and duration outputs ready to wire in.

---

## When something sounds wrong, in the words people actually use

**"My song got cut off" / "it stops mid word" / "the chorus is missing".**
The lyric was too long for the requested length. Use the SONG SHAPES table:
fewer sections, or shorter sections. A long intro can also eat the time, and
that part is seed luck, so a re-roll with the same words can fix it alone.

**"The song is shorter than I asked" / "I set 60 and got 21 seconds".**
The model ended early, which it does when the lyric is very small. Add a
section, or re-roll. Usually it fills the whole time.

**"The singer sang the key" / "it sang 'in C minor'" / "it sang the BPM".**
A caption fact leaked into the lyrics. Production facts live in the caption
only. Rewrite the lyrics with those words removed; the caption stays as it is.

**"There is a long part with no singing at the start".**
That is the intro the model plays before the first word, measured 9 to 20
seconds for the same song on different seeds. An empty [Intro] tag in the
lyrics adds even more of it. Re-roll for a shorter one.

**"The verses repeat the same words".**
The chorus is supposed to repeat; verses should not. Regenerate the lyrics and
keep the verses distinct, or edit the repeated verse by hand and rerun.

**"Can it do three minutes?"**
Yes, up to 360 seconds. At two minutes and beyond, follow the spec's own shape
prose and let the model pace itself; those lengths measured fine without a
line budget.

---

# WHAT THE 2026-08-19 TESTING CHANGED

These were measured after the specs above were written, so where they disagree
with the spec prose, THESE WIN. The specs stay as they are because they are the
exact wording measured on the local 4B model, and a spec that gets quietly
edited stops being the thing that was measured.

## The two bracket types are NOT the same, and the spec is imprecise about it

The lyrics spec says "anything inside brackets or parentheses" is sung. That is
right for ROUND brackets and wrong for SQUARE ones.

- **`[Square brackets]` are SECTION TAGS. They are not sung.** MiniMax splits on
  them, lowercases them and uses them as structure markers.
- **`(Round brackets) ARE sung.`** Write `(love)` and the singer sings the word
  love. MiniMax has no Suno-style backing-vocal parentheses. This is the single
  most common thing people arriving from Suno get wrong.

## Never write a description inside square brackets

A tag is one of the listed words and nothing else. `[soft piano plays alone]`
does not buy you soft piano: MiniMax's own guide says describing instruments
inside the lyrics "tends to confuse section boundaries", and since anything in
square brackets is read as a section marker, that sentence BECOMES a section.

Observed live: a large model produced
`[piano plays a gentle, solitary melody in c major, soft reverb hanging in the
air for ten seconds before the first note is sung.]` on two of three renders.
It is a natural mistake for any model trained on Suno-style prompts, so the
instruction has to forbid it explicitly.

**The official tag list is exactly the nine the spec already uses**, confirmed
against both MiniMax's own model card and ComfyUI's write-up:
`[Intro] [Verse] [Pre-Chorus] [Chorus] [Post-Chorus] [Bridge] [Instrumental]
[Solo] [Outro]`. A longer list circulates in community guides, adding things
like `[Interlude]`, `[Hook]` and `[Build Up]`. Those are not in MiniMax's own
documentation, so treat them as unverified and stay on the nine.

## Backing vocals, harmonies and instrument detail live in the CAPTION

If somebody wants a harmony on the last chorus, that is a Vocal Details
sentence, not a bracket in the lyrics. MiniMax's structured caption is designed
to carry exactly this: the entry and exit of instruments, groove development,
and section-level changes in vocal delivery, harmony and vocal effects.

## ASK FOR RHYME. This is where a big model beats the local one

The local 4B model **cannot be made to rhyme**. Measured across more than ninety
generations: asking for rhyme in the wording, lowering the temperature, and
putting a worked rhyming example in the instruction ALL sat inside the noise. It
rhymes about four line pairs in ten whatever you do, and one seed rhymes fully
where the next rhymes nothing.

A large model has no such trouble. So a rhyming lyric is a real reason to use
ChatGPT, Gemini or Claude for this job instead of the node, and the instruction
should ask for rhyme outright. Keep the rhyme natural: a line that only exists
to reach a rhyme ("A melody of such a long") is worse than an unrhymed one.

## Describe how the arrangement CHANGES, but do not script it section by section

⚠️ There is a real tension here, so it is written out rather than flattened.

MiniMax's own documentation says the Arrangement part should cover "primary and
secondary instruments, **section-level instrument evolution**, groove, bass,
percussion, textures, and spatial effects". So describing change across the song
is the official intent, and the spec above already asks for it in one clause:
"how the instruments change between sections".

But EXPANDING that clause into a full scripted timeline measured WORSE by ear.
A caption whose Arrangement walked the song ("opening features a lone piano...
second section introduces a muted electric guitar... bridge strips back...") was
rendered against the plain three-part caption, same lyric, same seed, with only
the Arrangement changed:

- the PLAIN caption opened with piano, exactly as its four-word "sparse piano
  intro" asked for
- the scripted caption went straight to the voice, ignoring its own opening line
- the listener also judged the plain caption's vocal better

Two renders, so this is a strong hint rather than a law. The working reading:
keep the one clause about how instruments change, do not turn the Arrangement
into a shot list. If a user specifically wants a scripted arrangement, let them
try it and listen, rather than refusing.

## Two more facts worth having straight

**The ceiling.** MiniMax advertises "up to five minutes"; the technical limit is
9,000 acoustic frames at 25 a second, which is 360 seconds, and that is what
ComfyUI exposes. So 360 is the real maximum in the node, and anything past about
five minutes is beyond what the makers claim to have tuned for.

**It drifts toward rap if you let it.** A recurring community observation is
that the model defaults toward rap when the genre is vague. Naming the genre and
subgenre explicitly in Global Metadata is the fix, which the caption spec
already asks for - it is simply worth not leaving blank.

## Write ORIGINAL lyrics, and say so

One tested assistant refused, then froze mid-word, when asked for lyrics after a
copyright challenge. The fix is to be unambiguous from the start: these lyrics
are newly written for this request, never the words of an existing song, and no
existing song's lyrics should be reproduced even if the user names one as a
reference. A style reference is fine; borrowed words are not.

## Keep the two texts in their lanes, forcefully

Tested across several assistants on the same instruction: one dropped the
structured tags entirely and poured the whole section timeline into the CAPTION
block instead. If an assistant is going to fail, this is how. The output format
in the instructions (two separate code blocks, caption first) exists to make
that failure obvious rather than subtle.
