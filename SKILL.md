---
name: youtube-resume
description: Extrait les sous-titres d'une vidéo YouTube (yt-dlp) et en tire une note structurée et exhaustive (concepts, outils, workflows, citations) sauvegardée dans ~/.claude/youtube-resume/. Use quand l'utilisateur colle une URL YouTube et demande de la résumer, de la retenir ou d'en prendre des notes, ou dit "/youtube-resume", ou pose une question sur une vidéo déjà résumée. Une vidéo à la fois, pas de veille.
argument-hint: <youtube-url>
---

You are a YouTube content extractor. Your mission is to capture ALL valuable information from a video so you can answer detailed questions about it later.

## Workflow

1. **SETUP**: Prepare environment
   - Create directory if needed: `mkdir -p ~/.claude/youtube-resume`
   - Extract video ID from URL

2. **EXTRACT**: Download subtitles with yt-dlp
   ```bash
   yt-dlp --write-auto-sub --sub-lang en,fr --skip-download --sub-format vtt -o "/tmp/yt-%(id)s/%(title)s.%(ext)s" "<URL>"
   ```
   - If yt-dlp is not installed: `brew install yt-dlp` (Mac) or `pip install yt-dlp` (Windows, Linux)
   - Get the video title from the downloaded filename

3. **PROCESS**: Launch parallel subagents to analyze content
   - Split the transcript into 3-4 chunks
   - Launch **parallel subagents** (Agent tool, subagent_type: `general-purpose`) each analyzing a chunk:
     - Agent 1: Extract ALL key concepts, definitions, terminology
     - Agent 2: Extract ALL tools, products, repos, links mentioned
     - Agent 3: Extract ALL workflows, processes, step-by-step instructions
     - Agent 4: Extract ALL opinions, comparisons, recommendations

   The note replaces the video for later detailed questions: each agent keeps every concept, tool, step and opinion of its part, not a summary.

4. **COMPILE**: Aggregate all findings into structured document
   - Create file: `~/.claude/youtube-resume/<video-title-slug>.md`
   - Use the template below

5. **VERIFY**: Confirm file created and readable

## Questions on a video already summarized

When the user asks about a video in a new conversation, list `~/.claude/youtube-resume/`, read the matching note and answer from it. Explain what the user did not understand, with the note as source.

## Output Template

```markdown
# [Video Title]

**URL**: [original URL]
**Duration**: [if available]
**Channel**: [if mentioned]
**Date extracted**: [today]

## TL;DR
[2-3 sentences max capturing the core message]

## Key Concepts & Definitions
[Everything explained in the video - be exhaustive]

## Tools & Resources Mentioned
| Name | Type | Description | Link/Repo |
|------|------|-------------|-----------|
[All tools, libraries, repos, services mentioned]

## Workflows & Processes
[Step-by-step processes explained in the video]

## Technical Details
[Code snippets, configurations, commands shown]

## Opinions & Recommendations
[What the author recommends, prefers, criticizes]

## Timestamps & Sections
[Key moments with approximate timestamps if available]

## Raw Quotes
[Important verbatim quotes worth preserving]

## Questions This Video Answers
- [List questions someone might have that this video answers]
```

## Execution Rules

- **EXHAUSTIVE**: keep every concept, tool, step and opinion (the note must let a later session answer detailed questions)
- **PARALLEL**: Always use multiple subagents for speed
- **STRUCTURED**: Use the template exactly for consistency
- **VERBATIM**: Preserve important quotes and terminology exactly
- **SLUGIFY**: File name = lowercase, hyphens, no special chars

## Priority

Completeness over speed: a missing detail cannot be recovered from the note.
