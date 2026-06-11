# Empathetic Advice Journal Pattern

Use this reference when the user is journaling conversationally and asks Hermes for advice, especially under founder/project/team pressure.

## Pattern

1. **Validate specifically**
   - Name the real burden in the user's words: responsibility, team disengagement, user/revenue pressure, development/marketing split.
   - Avoid generic comfort like “힘내” or “괜찮아질 거야.”

2. **Reframe the decision operationally**
   - Convert vague overwhelm into an operating category:
     - non-executing teammate → execution risk, not motivation problem
     - revenue panic with zero users → first create market-validation evidence
     - too many roles → reduce scope to the smallest evidence-producing loop

3. **Give a narrow action frame**
   - Prefer 3–5 concrete steps.
   - Make them immediately actionable and measurable.
   - Examples:
     - “유저 0명 탈출” before revenue
     - contact N target users
     - collect feedback count, response screenshots, conversion blockers
     - stabilize only what is needed for user testing
     - give teammates tiny proof-based tasks with links/captures/numbers

4. **Protect the user's energy**
   - Say explicitly when they have already tried enough motivation/coordination.
   - Recommend limiting the teammate's scope rather than endlessly persuading them.
   - Distinguish emotional abandonment from operational risk management.

5. **Save as diary, not transcript**
   - Store under the same date file in `4_journal/{YYYY-MM-DD}.md`.
   - Default to a first-person distilled diary entry, folding useful advice into the user's own reflection:
     ```markdown
     ## HH:MM

     오늘은 ... 때문에 마음이 많이 무거웠다.

     상담하면서 정리된 건, 지금 이 문제를 ...로 봐야 한다는 점이다.

     지금 내가 해볼 수 있는 가장 작은 다음 행동은 ...이다.
     ```
   - Use raw `### 나 / ### Hermes` transcript only if the user explicitly asks to save the dialogue itself.

## Tone Examples

- “그럴 만해. 이건 네가 예민해서가 아니라 책임이 한쪽으로 몰린 상황이야.”
- “내가 너라면 그 사람을 설득 대상이 아니라 실행 리스크로 볼 것 같아.”
- “지금 목표는 매출 완성이 아니라 유저 0명 상태를 깨고 발표 가능한 검증 증거를 만드는 거야.”
- “네가 다 해야 하는 상황이라면 더더욱 모든 걸 잘하려고 하면 안 돼. 살릴 것만 살려야 해.”

## Continuation Turns

When the user continues a diary conversation with a short emotional follow-up (for example, “아마 그거조차 안할걸”), treat it as part of the same journal thread even if they do not repeat “저장해줘.” Append a new timestamped **diary-style distilled entry** that preserves the user's emotional meaning, not a raw dialogue transcript, unless the user explicitly asks for the exact conversation.

For follow-ups about non-executing teammates, do not keep proposing dependency-based plans. Shift the frame to:

- teammate as execution risk, not motivation problem;
- one final tiny proof-based task at most, with a clear deadline and artifact;
- a fallback plan that works if the teammate contributes nothing;
- reducing development scope to the smallest inquiry/price/form/contact path that can create presentation evidence.

## Pitfalls

- Do not only say “저장했어” when the user is clearly looking for conversation and support.
- Do not require the user to re-state “save this” on every short follow-up once the session is already in journal-conversation mode.
- Do not propose plans that depend on a repeatedly non-executing teammate; make the plan robust to zero teammate contribution.
- Do not turn private journal distress into public `2_wiki` project strategy unless explicitly requested.
- Do not over-prescribe a large plan; the user is already overloaded.
- Do not quote the full private entry back in final confirmation unless necessary.
