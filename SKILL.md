---
name: conversational-journal-to-wiki
description: Use when the user writes a diary/journal entry conversationally in chat and wants it saved into the LLM-Wiki journal area. Default to diary-style first-person distillation under 4_journal, and do not mix private journal content into public wiki topics.
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [macos]
metadata:
  hermes:
    tags: [journal, diary, llm-wiki, obsidian, telegram, note-taking]
    related_skills: [llm-wiki, obsidian]
---

# Conversational Journal → LLM-Wiki Journal

## Overview

Use this skill when the user talks to Hermes as if writing a diary and expects the entry to be saved in the wiki's journal area.

The user's LLM-Wiki vault is:

```text
/Users/goyunseo/Library/Mobile Documents/com~apple~CloudDocs/LLM-Wiki
```

Journal notes live separately from general wiki topics:

```text
4_journal/{YYYY-MM-DD}.md
```

Core principle: **make the saved note feel like a diary, not a chat transcript**. Preserve the user's diary voice, emotional truth, and concrete details, but by default distill the conversation into a first-person journal entry rather than storing alternating `나 / Hermes` turns.

Default mode for this user is **diary-only saved, counseling in chat**:
- Save only the user's diary content in the journal file, written in a readable first-person Korean diary style.
- Do not save Hermes's advice, validation, reframing, counseling, or response into the journal unless the user explicitly asks to save Hermes's response too.
- Do not save long mechanical plans, tool logs, or generic advice.
- In the chat reply, Hermes should still respond like a trusted counselor/companion: validate the user's feelings and reflect the situation before offering advice.
- If the user says advice, strategy, or “how-to” suggestions feel draining, **stop advising**: respond with concise validation/recognition only, and save the reflection as journal if appropriate. Do not reframe it as a PM/action-plan problem unless the user explicitly asks.
- Use raw `### 나` / `### Hermes` transcript format only when the user explicitly asks for “대화 그대로 저장”, “너 답변도 저장”, or equivalent.

User-specific automation preference:
- Treat long, reflective Telegram messages from Ko Yunseo as journal-worthy by default, even when they do not explicitly say “일기” or “저장”. This includes career anxiety, school/exam stress, money/investing worries, project/team burnout, identity/self-worth reflections, decisions, conflicts, and long emotional updates.
- Auto-save these to `4_journal/{YYYY-MM-DD}.md` unless the user explicitly says not to save (`기록하지 마`, `저장하지 마`, `오프더레코드`, etc.).
- For each saved entry, generate a short Obsidian-friendly title and tags. Title should be specific and human-readable; tags should include emotions, events, domains, and project names when present.
- Keep this automatic journal write-back separate from `2_wiki/` topic pages. Do not turn private emotions into public project wiki facts unless the user explicitly asks.

Preferred interaction style: when the user sends a diary-like message conversationally, respond like a trusted conversation partner first — warm, validating, and specific to what they said — while still saving a diary-like distilled entry. For this user, avoid replies that feel like a report or template: do not default to headings like `현재 판단`, `P0/P1/P2`, numbered action plans, or repeated “상황 요약 → 판단 → 액션” structure during emotional disclosure. Use more human, direct language; reflect the specific unfairness, exhaustion, anger, or sadness in plain Korean before offering any plan. If the user signals that advice itself feels draining, says they are too exhausted for solutions, or says the reply feels “형식적”, **stop advising**: acknowledge the mismatch, apologize briefly, and continue with concise, emotionally present validation. Save the reflection as a journal entry, but do not make the chat response about the mechanics of saving unless the user asks. For advice-heavy journal conversations or specific topics (finance, team stress, etc.), follow the general interaction style described above. Specific patterns are not required unless the user explicitly requests detailed handling.

## When to Use

Trigger on requests like:

- "이거 일기로 저장해줘"
- "오늘 일기"
- "위키에 일기로 넣어줘"
- "내 일기장에 저장해줘"
- The user writes a personal reflection in a conversational style and indicates it should be saved as a diary/journal entry.

Do not use this skill for:

- General source/wiki ingest requests → use `llm-wiki` INGEST.
- Project notes, meeting summaries, screenshots, or research docs unless the user explicitly says they are diary entries.
- Private credentials, tokens, passwords, or secrets. Never store or expose secrets.

## Required Companion Skills

Before acting, load:

1. `llm-wiki` — for the `4_journal/` separation rules.
2. `obsidian` — for filesystem-safe vault reading/writing.

## Workflow

1. **Identify the diary text**
   - If the user clearly separates instruction and diary text, save only the diary text.
   - If the entire message is a diary-like entry plus a short instruction, remove only the instruction phrase and preserve the rest.
   - If it is ambiguous what part is diary text, ask one short clarification.

2. **Get the current local date**
   - Use `terminal` with `date +%F`.
   - Never guess the date from memory.

3. **Ensure journal directory exists**
   - Target directory:
     ```text
     /Users/goyunseo/Library/Mobile Documents/com~apple~CloudDocs/LLM-Wiki/4_journal
     ```
   - Create it if missing.

4. **Create or append to the date file**
   - File path:
     ```text
     /Users/goyunseo/Library/Mobile Documents/com~apple~CloudDocs/LLM-Wiki/4_journal/{YYYY-MM-DD}.md
     ```
   - If the file does not exist, create it with YAML frontmatter:
     ```yaml
     ---
     date: YYYY-MM-DD
     origin: telegram
     source: conversational-journal
     tags: [일기]
     ---

     # YYYY-MM-DD
     ```
   - If the file exists, append a new entry rather than overwriting previous entries.

5. **Entry format**
   - Default for Ko Yunseo: append a timestamped **diary-style distilled entry** with an entry title and Obsidian tags:
     ```markdown
     
     ## HH:MM — 짧은 제목

     tags: #감정/불안 #사건/시험 #프로젝트/UniPort

     오늘은 ...

     지금 가장 크게 느끼는 건 ...

     상담하면서 정리된 생각은 ...

     그래서 지금은 ...부터 해보려고 한다.
     ```
   - The saved entry should read like the user's own diary, not a chat log.
   - Preserve Korean, emotional nuance, concrete events, and important user phrases, but lightly rewrite for diary readability.
   - Generate the title yourself. Good titles are concrete and compact, e.g. `유니포트 이후 번아웃`, `시험 앞에서 멈춘 하루`, `커리어 불안과 비교감`, `돈과 미래 압박`.
   - Generate tags yourself. Prefer Obsidian-friendly slash tags grouped by class:
     - Emotions: `#감정/불안`, `#감정/무기력`, `#감정/열등감`, `#감정/분노`, `#감정/슬픔`, `#감정/압박감`, `#감정/회복`
     - Events: `#사건/시험`, `#사건/공모전`, `#사건/지원사업`, `#사건/현장실습`, `#사건/팀프로젝트`, `#사건/투자손실`
     - Domains: `#영역/진로`, `#영역/공부`, `#영역/돈`, `#영역/건강`, `#영역/관계`, `#영역/프로젝트`
     - Projects/people: `#프로젝트/UniPort`, `#프로젝트/AI유즈케이스`, `#프로젝트/MarketFlowSentinel`
   - Add file-level frontmatter tags only for broad categories (`[일기, telegram]` by default). Put entry-specific emotion/event tags under each entry so multiple entries in one day can differ.
   - Do not include a `### Hermes` section by default. Fold useful advice into “상담하면서 정리된 생각”, “지금 해볼 것”, or “내가 놓지 말아야 할 기준”.
   - If the user explicitly says “대화 그대로 저장”, “상담 내용 전체 저장”, or “너 답변도 같이 저장”, then use raw transcript format:
     ```markdown
     ## HH:MM

     ### 나
     <user diary text exactly as written>

     ### Hermes
     <empathetic response>
     ```
   - When the user asks advice inside the journal conversation (e.g. “너라면 어떻게 할거야”, “이걸 어떻게 해야할까”), answer in chat as both companion and operator, but save the journal as a first-person reflection plus a short action frame.
   - For overwhelming project/team stress, prioritize narrowing the objective into evidence-producing next steps (e.g. “유저 0명 탈출”, contact counts, feedback, conversion blockers, next experiment) and protect the user's energy by reducing reliance on non-executing teammates.
   - For personal finance / investing reflections, respond as a grounded thinking partner, not as a trade caller. Use simple arithmetic to clarify feasibility, distinguish labor cashflow from investment income, explicitly avoid telling the user to buy/sell a security, and convert leverage/FOMO into a risk-frame: protect the core goal capital, cap any aggressive allocation, require entry/exit/loss rules, and separate repeatable skill from market luck. Save the journal entry as private `4_journal` material, not a public investing wiki topic, unless the user explicitly asks otherwise.

6. **Keep journal separate**
   - Do not update `2_wiki/index.md`.
   - Do not update `log.md` unless the user asks for audit logging.
   - Do not compile the diary into topic pages.
   - Do not graphify/index `4_journal/`.

7. **Verify**
   - Read back or inspect the target file after writing.
   - Confirm the new entry appears in the correct date file.

8. **Reply briefly in Korean**
   - Say the diary was saved.
   - Include the path or filename.
   - Do not quote private diary text back unless useful and short.

## Implementation Pattern

Use file tools when possible:

- `read_file` to inspect an existing date file.
- `write_file` to create a new date file or rewrite with appended content.
- `terminal` only for `date`, `mkdir -p`, or existence checks when file tools are insufficient.

Example sequence:

1. `terminal(command="date '+%F %H:%M'")`
2. `terminal(command="mkdir -p '/Users/goyunseo/Library/Mobile Documents/com~apple~CloudDocs/LLM-Wiki/4_journal'")`
3. Try `read_file(path=target)`.
4. If missing, `write_file(path=target, content=new_file_content)`.
5. If existing, append by reading full content and writing full updated content, or use `patch` with a stable trailing anchor.
6. Re-read the final lines to verify.

## Privacy and Safety Rules

- Treat diary text as private.
- Do not send it to external services.
- Do not reveal diary content in the final reply unless the user requests it.
- Do not store credentials, tokens, passwords, API keys, or keychain contents.
- Do not infer sensitive facts into durable memory from diary entries unless the user explicitly asks to remember a stable preference/fact.

## Common Pitfalls

1. **Saving a chat transcript when the user wanted a diary**
   - Default to diary distillation: first-person, reflective, readable, with advice integrated as the user's own thinking.
   - Use raw `나/Hermes` transcript only when explicitly requested.

2. **Flattening the diary into a generic summary**
   - Preserve the user's emotional voice, concrete situation, and distinctive phrases. Distill, don't sanitize.

3. **Putting diary material into `2_wiki/`**
   - Personal journal stays in `4_journal/`.

4. **Creating one file per message timestamp**
   - Use one date file and append multiple timestamped entries.

5. **Guessing the date**
   - Always call `date`.

6. **Echoing private content back in chat**
   - Keep the response short: saved + path.

7. **Overwriting an existing journal day**
   - Always read first; append if the file exists.

## Verification Checklist

- [ ] Loaded `llm-wiki` and `obsidian` when saving an actual entry.
- [ ] Used live local date/time from `date`.
- [ ] Target path is under `LLM-Wiki/4_journal/`.
- [ ] Existing date file was appended, not overwritten.
- [ ] Diary entry reads like a first-person journal, not a chat transcript; raw dialogue is used only when explicitly requested.
- [ ] Did not update `2_wiki`, `index.md`, or graph indexing.
- [ ] Re-read/verified the file after writing.
- [ ] Final reply is concise Korean with saved filename/path.
