# conversational-journal-to-wiki

Hermes Agent skill that turns conversational diary/reflection messages into clean, first-person journal entries in the LLM-Wiki `4_journal/` area.

Keeps private emotional content completely separate from public wiki topics.

## Features
- Automatic detection of long reflective messages
- Saves as dated, tagged Markdown entries
- First-person diary style (not chat logs)
- Works especially well with Telegram long messages
- No reference patterns required (fully self-contained)

## Installation
1. Place `SKILL.md` in your Hermes skills folder:
   ```
   ~/.hermes/skills/note-taking/conversational-journal-to-wiki/SKILL.md
   ```
2. Customize the vault path and any user-specific triggers inside `SKILL.md` if needed.

## Usage Examples

### Example 1: Post-exam relief (real case)
**User message:**
> 드디어 시험이 끝났다. 그동안 사실 너무나도 많은일이 있고 감정이 있고... (긴 반성 + 휴학 결정 + 사람 스트레스 + 주식 욕심 + 휴학 목표 나열)

**Result:**
- Saved to `4_journal/2026-06-20.md`
- Title: `시험이 끝난 후의 시원함과 휴학을 앞둔 자유`
- Tags: `#감정/회복 #사건/시험 #영역/진로 #영역/돈`
- Hermes reply: "시험이 끝나서 정말 시원하시겠어요. ... 일기장에 저장했어요: 4_journal/2026-06-20.md"

### Example 2: Money & future anxiety
**User message:**
> 주식도 엄청 올라서 돈에 대한 욕심이 엄청 생겼다. 올해 주식으로 1억 모으고 싶고...

**Result:**
- Saved with tags `#감정/욕심 #영역/돈`
- Clean first-person entry without any Hermes advice mixed in

### Example 3: Explicit request
**User:** "이거 일기로 저장해줘" + long text

→ Immediately triggers journal save with the provided content.

## Recommended Companion Skills
- `llm-wiki`
- `obsidian`

## Note
This skill is designed to be general-purpose. After installing, review the "User-specific automation preference" section in `SKILL.md` and adapt it to your own triggers and vault path.