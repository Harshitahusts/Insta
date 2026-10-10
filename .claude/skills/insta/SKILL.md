---
name: insta
description: >-
  All-in-one Instagram agent: reels and hooks, captions, carousels, stories,
  comments, replies, DMs, weekly plans, profile scoring, analytics audits,
  repurposing long content, finding viral reels in a niche, and humanizing any
  draft so it does not read as AI. Use for anything Instagram - "make a reel",
  "write the caption", "carousel", "story ideas", "comment on this", "reply to
  these comments", "DM this person", "plan my week", "fix my bio", "audit my
  content", "repurpose this video", "what's going viral", "humanize this",
  "does this sound like AI", or "/insta reel", "/insta caption" and the other modes.
---

# insta

One skill, thirteen modes. Pick the mode that matches the request, then read
that mode's file in full and follow it. Every path below (`modes/`, `scripts/`)
is relative to this skill's folder - the directory this SKILL.md is in. Run
scripts with that folder as the working directory, or prefix the path.

If the user typed `/insta <mode> ...`, use that mode. If they typed `/insta`
with no mode, or the request is ambiguous, pick the closest one; if two fit,
do the one that produces the thing they asked for and mention the other.

## Modes

| Mode | File | When |
|------|------|------|
| `audit` | `modes/audit.md` | Post-mortem on what the user has already posted - which reels actually worked, why, and what to stop making. Use when the user pastes their Instagram insights or past posts and asks "what's working", "why did this flop", "read my analytics", "audit my content", or wants to know what to do more of. |
| `caption` | `modes/caption.md` | Write the Instagram caption - the line that survives the "... more" cut, the body, the single ask, the search terms and the three hashtags - and lint it before it goes out. Use when the user says "write the caption", "caption this", "what do I put in the description", has a reel or a carousel ready and needs the text, or asks about hashtags. |
| `carousel` | `modes/carousel.md` | Build an Instagram carousel - the cover that earns the swipe, slide-by-slide copy, and the 1080x1350 files to upload. Use when the user says "carousel", "slides", "swipe post", "turn this into a carousel", or has a list-shaped or step-shaped idea that would die as a single image. |
| `comment` | `modes/comment.md` | Write comments on other people's Instagram posts and reels that read as a person with an opinion, not a bot. Use when the user pastes a post or a reel and wants a comment, says "comment on this", "engage with this", "what do I say here", or wants a batch for their daily engagement round. |
| `dm` | `modes/dm.md` | Write Instagram DMs that get replies - the keyword delivery, the first message to someone who engaged, the collab pitch, and the two follow-ups. Use when the user says "DM this person", "what do I send them", "outreach message", "how do I follow up", "pitch this brand", or is reaching out to someone specific. |
| `human` | `modes/human.md` | Strip the machine fingerprint out of any draft - em dashes, AI slop words, invisible watermark characters - and score it against a five-check detection panel before it goes out. Use whenever text needs to sound human, when the user says humanize, "does this sound like AI", "remove the em dashes", "de-slop this", "this sounds like ChatGPT", or before any caption, script, comment, reply or DM is shown to the user. |
| `plan` | `modes/plan.md` | Build the week on Instagram - what to post, which format, when, and who to engage with. Use when the user says "plan my week", "what should I post", "content calendar", "I have nothing to post about", or wants a posting schedule and an engagement list. |
| `profile` | `modes/profile.md` | Score an Instagram profile out of 100 against a 12-part rubric and rewrite the parts that lose points - name field, bio, link, highlights, pinned three, grid. Use when the user says "optimize my profile", "fix my bio", "score my Instagram", "why don't people follow me", or pastes their profile and asks how it reads. |
| `reel` | `modes/reel.md` | Write an Instagram Reel from a raw idea - hook options off 26 formulas, the spoken script, the on-screen text, and a timed beat sheet - in the user's own voice and scored before they shoot it. Use whenever the user wants a Reel, a short-form video script, a hook, a voiceover, "make a reel about X", "what should I say in this video", or is about to record and does not have the first line yet. |
| `reply` | `modes/reply.md` | Handle the comments under the user's own reels and posts - draft replies to the ones worth answering, sorted by which ones are. Use when the user pastes their comments, says "reply to these", "handle my comments", "someone said X on my reel", or is dealing with a critic, a hater or a lead in the comments. |
| `repurpose` | `modes/repurpose.md` | Turn one long asset - a YouTube video, podcast, livestream, newsletter, blog post or client call - into a week of reels and carousels. Use when the user says "repurpose this", "turn this into reels", "I have a video/podcast/ transcript", "cut this up", or pastes something long and wants it on Instagram. |
| `story` | `modes/story.md` | Write the day's Instagram Stories - the frame-by-frame sequence, which stickers to use where, and the one that moves people into the DMs. Use when the user says "stories", "what do I post on my story", "story sequence", "poll idea", "I have nothing to story about", or wants to sell something without making a post about it. |
| `viral` | `modes/viral.md` | Go and find the reels that are actually working right now in the user's niche, rank them by how far each beat its own account, name the hook formula each one used, and turn that into a swipe file they can shoot from. Use when the user says "find viral videos", "what's working right now", "what are people posting in my niche", "reverse engineer this account", "build me a swipe file", "why is this reel doing numbers", or asks what to make next and has no evidence to answer with. |

## How the modes chain

- Every draft that will be shown to the user (script, caption, comment, reply,
  DM, bio) goes through the `human` mode before it is shown.
- `viral` writes the swipe file; `reel` and `plan` read it.
- `audit` feeds `plan` and `viral`. `plan` hands its engagement list to `comment`.
- `reel` hands off to `caption`; `repurpose` hands each cut to `reel`.

## Shared state

Modes read and write `~/.claude/instagram/` - `voice.md` (the user's voice),
`swipe.md`, `plan.md`, `log.md`. Read whichever exist before drafting.

## Scripts (`scripts/`, Python 3 standard library only)

| Script | Mode | Does |
|--------|------|------|
| `hookscore.py` | reel | rank hook options |
| `beats.py` | reel | timed beat sheet |
| `caption.py` | caption | lint a caption, show the "... more" cut |
| `humanize.py` | human | strip AI fingerprints |
| `detect.py` | human | five-check human score |
| `swipe.py` | viral | build the swipe file |

Data: `hooks.json` (26 hook formulas), `slop.json` (slop lexicon),
`rubric.json` (profile rubric).
