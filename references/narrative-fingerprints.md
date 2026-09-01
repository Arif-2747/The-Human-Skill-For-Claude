# Model fingerprints

From StoryScope, section 5 and Table 17. Each model has a set of narrative features where it diverges from the other four.

## Read this first

Six-way attribution scored 68.4 macro-F1 on narrative features alone. Human-versus-AI scored 93.2. Telling *which* model wrote something is much harder than telling that a model wrote it.

Gemini, DeepSeek, and Kimi get confused with each other constantly. The six most-confused pairs in the whole study are all AI against AI, never human against AI.

So: treat a model guess as a lean. Never state it as fact.

Number of distinct fingerprint features found per source: Human 32, Claude 26, GPT 11, Gemini 11, DeepSeek 7, Kimi 3.

---

## Claude Sonnet 4.6

The most distinctive of the five, and the easiest to spot.

The defining quality is restraint. Event intensity escalates less than in any other source. Narrative voice stays the most uniform throughout.

- Flattest event escalation of all six sources
- Lowest event-type diversity
- Writes epilogues and flash-forward endings more often
- Avoids dream sequences
- Takes a reverent approach to literary tradition, honouring and extending conventions rather than subverting them, in 62% of stories against 39 to 56% for the others
- Prefers quiet endings over avalanche endings

**In practice.** A Claude story is careful and consistent and never quite peaks. If a script builds and builds and then settles gently, and the voice never shifts register, that is the tell.

---

## GPT-5.4

Socially oriented. The most people-watching of the five.

- Gossip and rumour as a plot mechanism in 64% of stories against 44 to 55% for the others
- Frames stories as looking back on events from years or decades ago
- Ensemble-heavy social networks, at roughly human levels
- Subverts reader expectations more than the other models, 41% against 27 to 36%
- Leaves reconciliations partial or ambiguous

**In practice.** Narrator telling you about a thing that happened a long time ago, with a whole town's worth of people talking about it.

---

## Gemini 3 Flash

- Bleakest settings of all six, 88% tagged bleak and oppressive
- Tidiest endings and the longest extended denouements
- Protagonist's social circle expands over the story
- Speech rendered as direct dialogue rather than reported
- Reaches for siege and ordeal story shapes
- Frequent flashbacks

**In practice.** Dark setting, everything wraps up neatly, and it keeps going after it should have ended.

---

## DeepSeek V3.2

- Front-loads crucial context that other sources hold back
- Highest narrator presence and visibility
- Emotion shown through behavioural cues
- Backstory spread evenly through the piece rather than clustered
- Leans atmosphere over plot
- Uses embedded storytelling, stories inside the story

**In practice.** Tells you everything up front, then narrates loudly.

---

## Kimi K2.5

Only three fingerprint features. The lowest attribution score of any source. It sits at the generic centre of the AI distribution with no distinctive narrative choices of its own.

- Introduces characters mid-action
- Opens in medias res
- Avoids labelling character traits explicitly

**In practice.** If a piece reads as AI but has no other model's signature, Kimi is the default guess. Weak evidence.

---

## Human

32 fingerprint features, the most of any source. Top five by uniqueness:

- Introduces characters through dialogue
- Single focal character rather than shifting focus
- Narrator does not address the reader directly (note: this contradicts the group-level finding that humans address the reader more, because this is a *fingerprint* feature measuring what separates humans from the AI cluster on a different axis; do not use it as a writing rule)
- Back-loads revelations
- Crosses genre boundaries

Plus subplot density, unusual twist placement, naming practices, and how visibly information is withheld.

Human is the easiest class to identify in the whole study, at 88.5% F1 on narrative features alone.
