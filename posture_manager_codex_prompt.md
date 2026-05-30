# Codex Implementation Prompt: Floating Posture Manager MVP

You are a senior frontend developer and product-minded builder.
Your task is to complete a working MVP within 2 hours.
Do not stop to ask questions. Make reasonable decisions and implement the app end to end.

Build a React + Vite web app that can run directly in the Codex environment.

## Product Goal

Create a floating health manager MVP for people who work in front of a computer for long hours.

The app should feel like a small manager widget that a developer can keep beside their coding window.
It reminds the user to lift their head, stretch their neck and shoulders, stand up, and move after meals.

The core idea:

- A small character manager is shown in a floating widget.
- If the user sits too long, ignores reminders, or keeps skipping breaks, the character gradually becomes rounder, heavier, and more tired.
- If the user completes stretches or stands up, the character becomes lighter, brighter, and healthier again.
- This should feel encouraging, not blaming.

The app is not just a timer.
The important product loop is:

> My work habits affect the character's condition, and the character gently helps me notice my own body.

## Product Principles

- Do not shame the user.
- Do not use harsh language about body shape, weight, or failure.
- The character should feel like a small supportive coworker.
- The tone should be: "Let's get healthier together."
- The MVP should prioritize working behavior over excessive polish.
- It must be runnable, buildable, and visually complete.

## Tech Stack

- React + Vite
- Plain CSS
- No backend
- No login
- No database
- Use localStorage for today's records and settings
- The app must run with:
  - `npm install`
  - `npm run dev`
  - `npm run build`

## File Structure

Keep the structure simple.

Recommended structure:

- `package.json`
- `index.html`
- `src/main.jsx`
- `src/App.jsx`
- `src/App.css`
- `README.md`

## Main UI

Build the app as a small floating widget, not a full landing page.

Requirements:

- The widget width should be around 340px to 380px on desktop.
- On mobile, it should fit within the viewport without layout breaking.
- The widget should look like something that could sit beside a coding window.
- Use a calm desktop/workspace background behind the widget.
- The visual style should be calm, soft, and modern.
- Avoid a childish toy-like look.
- Use a light mode base.
- Use off-white or soft gray background colors.
- Use mint or green as the main positive accent.
- Use coral or peach for warning/tired states instead of aggressive red.

The widget should include:

- Character
- Current character condition
- Timer display in `mm:ss`
- Speech bubble or small reminder message
- Action buttons
- Small condition bar
- Today's records
- Settings panel

## Character

Implement the character with HTML/CSS. A separate SVG file is not required.

The character should have:

- Rounded body
- Face
- Expression
- Small arms or legs
- Visual state changes

Character states:

- `energised`: bright, light, energetic
- `normal`: stable and okay
- `stiff`: slightly tense or sore
- `heavy`: rounder and heavier
- `tired`: slumped and exhausted

State changes should be visible.

As the state gets worse:

- Body becomes slightly larger and rounder
- Posture looks more slumped
- Face looks more tired
- Motion becomes slower or less energetic

As the state improves:

- Body becomes lighter
- Expression becomes brighter
- Posture becomes more upright

Use CSS transitions or animations for state changes.
If quiet mode is enabled, reduce stronger animation effects.

## Health Score

Use `healthScore` as the main state driver.

Rules:

- `healthScore` is an integer from `-10` to `+10`.
- Clamp all score changes to this range.
- First app launch default: `0`.
- Daily reset default: `0`.
- Initial character state: `normal`.

Map score to character state:

| healthScore | characterState |
| --- | --- |
| `+6` to `+10` | `energised` |
| `+1` to `+5` | `normal` |
| `0` to `-2` | `stiff` |
| `-3` to `-6` | `heavy` |
| `-7` to `-10` | `tired` |

Do not display the score as the main UI focus.
Show the condition mainly through the character and a small condition bar.
It is okay to show a small label like "컨디션 점수".

## Timer

The default work reminder interval is 50 minutes.

For testing, the settings panel must allow:

- 10 seconds
- 30 seconds
- 1 minute
- 50 minutes

Display the remaining time as `mm:ss`.

When the timer reaches zero:

- Enter reminder mode.
- Show a speech bubble.
- Show action buttons.
- Pause the countdown while the reminder is waiting for user action.

## Automatic Decline When Ignored

This is a core product behavior.

If the reminder is visible and the user does not click anything:

- In 50-minute mode, wait 2 minutes.
- In test modes such as 10 seconds, 30 seconds, or 1 minute, wait 10 seconds.
- Then automatically treat it as an unanswered skip.

Automatic unanswered skip behavior:

- `skipCount += 1`
- `healthScore -= 1`
- Character state becomes worse if the score crosses a threshold
- Show a gentle message
- Restart the work timer

This means that if the user ignores reminders and keeps working, the character gradually becomes heavier and more tired.

## Actions

When a reminder is shown, display these buttons:

- `완료`
- `5분 뒤`
- `스킵`
- `일어났어요`

### Complete Button

When the user clicks `완료`:

- `completeCount += 1`
- `healthScore += 1`
- Show a positive speech bubble
- Restart the timer

Example message:

- `좋아요! 움직였더니 훨씬 가벼워졌어요.`

### Snooze Button

When the user clicks `5분 뒤`:

- `snoozeCount += 1`
- Do not change healthScore
- In real 50-minute mode, remind again after 5 minutes
- In test modes, remind again after half of the selected interval
- Show a gentle message

Example message:

- `괜찮아요. 조금 뒤에 다시 살짝 알려줄게요.`

### Skip Button

When the user clicks `스킵`:

- `skipCount += 1`
- `healthScore -= 1`
- Show a soft encouragement message
- Restart the timer

Example message:

- `우리 조금 뻐근해졌어요. 다음엔 30초만 같이 움직여요.`

### Stand Button

When the user clicks `일어났어요`:

- `standCount += 1`
- `healthScore += 2`
- Show a stronger recovery message
- Restart the timer

Example message:

- `좋아요. 일어났더니 몸이 훨씬 가벼워졌어요.`

## Reminder Message Examples

Use Korean copy.
Avoid blame, guilt, or body-shaming.

Examples:

- `50분 지났어요. 고개 한번 들어볼까요?`
- `오래 앉아 있었어요. 같이 한번 일어나요.`
- `우리 조금 뻐근해졌어요. 30초만 움직여요.`
- `어깨가 굳었어요. 천천히 돌려볼까요?`
- `좋아요! 움직였더니 훨씬 가벼워졌어요.`
- `괜찮아요. 5분 뒤에 다시 살짝 알려줄게요.`
- `점심 먹고 바로 앉기 전에 5분만 걸어볼까요?`

## Reminder Priority

Only show one active reminder at a time.

Priority:

1. Meal walk reminder
2. 50-minute work break reminder
3. General encouragement message

If a new reminder becomes eligible while another reminder is active, keep the current reminder and check again in the next cycle.

## Today's Records

Show a compact "today" section with:

- 완료 횟수
- 스킵 횟수
- 미루기 횟수
- 일어나기 횟수
- 현재 상태

Use localStorage so the data persists across reloads.

## Daily Reset

Store `lastActiveDate`.

Rules:

- On app load, compare today's date with `lastActiveDate`.
- If the date changed, reset today's records.
- If the app stays open across midnight, check the date every 1 minute.
- If the date changes, reset today's records.

Daily reset should reset:

- `healthScore` to `0`
- `completeCount` to `0`
- `skipCount` to `0`
- `snoozeCount` to `0`
- `standCount` to `0`
- meal reminder shown flags

Settings should not reset.

## localStorage

Use this key:

```js
postureManager_v1
```

Suggested data shape:

```js
{
  lastActiveDate,
  healthScore,
  completeCount,
  skipCount,
  snoozeCount,
  standCount,
  mealReminderShown,
  settings
}
```

`mealReminderShown` can contain:

```js
{
  lunch: false,
  dinner: false
}
```

## Settings Panel

Add a simple settings panel inside the widget.

Settings:

- Reminder interval:
  - 10 seconds
  - 30 seconds
  - 1 minute
  - 50 minutes
- Quiet mode toggle
- Widget size:
  - compact
  - normal
- Lunch reminder toggle
- Lunch time input
- Dinner reminder toggle
- Dinner time input
- Reset data button

Persist settings in localStorage.

## Quiet Mode

Quiet mode behavior:

- Reduce large animations and strong visual effects.
- Do not show overly prominent speech bubble styling.
- Use a smaller, calmer reminder indicator.
- Timer and state changes must still work.
- Do not implement sound notifications in the MVP.

## Meal Reminder

Implement a simple MVP meal reminder.

Rules:

- Lunch and dinner reminders are optional toggles.
- Each meal reminder should appear at most once per day.
- Show a meal reminder only within 30 minutes after the configured meal time.
- Example: if lunch time is `12:30`, show it only between `12:30` and `13:00`.
- If the user clicks `완료` for a meal reminder, treat it like a walk completion:
  - `standCount += 1`
  - `healthScore += 2`
  - mark that meal reminder as shown today
- If the user clicks `스킵`, mark that meal reminder as shown today.
- Do not repeatedly show the same meal reminder.
- Do not use browser notification permissions.

## Implementation Priority

Mandatory:

1. React/Vite app runs
2. Floating widget UI
3. Timer with test intervals
4. Reminder speech bubble
5. Buttons: `완료`, `5분 뒤`, `스킵`, `일어났어요`
6. `healthScore` and character state mapping
7. Visible character state changes
8. Automatic unanswered skip behavior
9. localStorage persistence
10. Daily reset
11. README
12. `npm run build` succeeds

Optional if time remains:

1. Meal reminder
2. Quiet mode polish
3. Compact/normal visual refinement
4. More detailed character animations

If time is short, do not leave the mandatory features half-done in order to polish optional features.

## README

Write a clear `README.md` with:

- App summary
- Features
- How to run
- How to test quickly with 10-second interval
- What is stored in localStorage
- Known limitations

## Completion Requirements

Before finishing:

- Run `npm install` if dependencies are missing.
- Run `npm run build`.
- If possible, run `npm run dev` and provide the localhost URL.
- Check that the app does not show a blank screen.
- Check that the timer, buttons, score changes, and character state changes work.
- Check that there are no obvious console or build errors.

Final response should include:

- What was implemented
- How to run it
- Build result
- Any remaining limitations

