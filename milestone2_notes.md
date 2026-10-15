# Milestone 2 — Label Taxonomy

## Labels

### `analysis`

**Definition:** An evaluative claim about a show that is backed by a specific, checkable detail (a scene, episode, stat, or named fact) which is explicitly connected to the verdict by reasoning language.

**Example 1:**
> "It seems to me that they don't want Muppets to do well, they want them to do astronomically well. Like, the 2011 movie did well at the box office, but the sequel and the attempts at tv shows don't find a massive audience like other IPs."

**Example 2:**
> "I had to stop when the Walking Dead they got to that junkyard town with the lady with the bad haircut. I think they were called the Scavengers. By that point, all my favorite characters were gone and I didn't see the point to keep on going."

### `take`

**Definition:** A confident evaluative claim about a show with no specific support offered, or a specific detail mentioned without being used to argue the verdict.

**Example 1:**
> "Malcolm in the Middle has aged really well, though i think my name is earl has a plenty of charm too."

**Example 2:**
> "Just going to suggest Peter Jackson makes Muppets LOTR. He has experience directing with Muppets and it would be absolutely legendary!"

## Inclusion threshold (scoping rule, applied before labeling)

A post is in scope if it states a preference, recommendation, or evaluative judgment about the show/episode. Posts that only report an action (watching, started, finished) with no judgment attached are excluded, even if about a show. (Posts reacting to real-world news about a person, unrelated to the show's quality, are also excluded — see Milestone 1 notes.)

## The hardest boundary

**Which two labels:** `analysis` vs. `take`

**The boundary case:**
> "I love when Dewey tells the bullies he called their moms and while they scoff, their mothers show up and they get dragged off by their mothers."

This names an exact, checkable scene — the reason `analysis` exists as a label. But naming a scene isn't the same as arguing something with it; "I love when X happens" is a feeling with a detail attached, not a claim built from that detail.

**Decision rule:** A specific, checkable detail only makes a post `analysis` if it's explicitly connected to the verdict by reasoning language — "because," "that's why," "and it shows," "by that point," etc. If the detail and the opinion sit side by side with no stated causal link, it's `take`.

Applying this rule: the Dewey comment is `take` — no causal link between the scene and any broader verdict about quality.

## Stress test (8 AI-generated boundary posts)

| Post | Label | Why |
|---|---|---|
| "The writing fell off a cliff after season 3 — they swapped out two of the original writers, and it shows." | analysis | "and it shows" links the fact to the verdict |
| "Season 3 was mid, ngl." | take | no detail, no support |
| "This show nails comedy because every episode is under 22 minutes — nothing overstays its welcome." | analysis | "because" links detail to verdict |
| "This is easily one of the best shows ever made." | take | no support |
| "I still think about the scene where the two brothers finally talk at the diner. Gets me every time." | take | detail named, no causal link to a verdict |
| "Ratings dropped from 8M to 3M after they killed the lead in episode 5 — that's why it got cancelled." | analysis | "that's why" links stat to verdict |
| "Everyone loves the pilot but honestly it's overrated." | take | no support |
| "Every time I watch this show I end up more impressed, especially with the lighting in episode 4." | take | detail named, no causal link stated |

All 8 classified cleanly under the tightened decision rule. Initial (untightened) rule left one ambiguous (the lighting/episode 4 post); adding the "explicit reasoning language" requirement resolved it.
