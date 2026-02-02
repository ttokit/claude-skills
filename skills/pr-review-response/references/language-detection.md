# Language Detection

Detect the primary language for PR comment replies based on PR metadata.

## Basic Rules

| Content Pattern | Detected Language |
|-----------------|-------------------|
| Japanese characters (hiragana/katakana/kanji) | Japanese |
| English/Latin characters | English |

## Edge Cases

### Mixed Content

When PR title/body contains both Japanese and English:
- Use AskUserQuestion to let user choose
- Options: "Japanese" / "English"

### Empty or Minimal Content

When PR title/body is empty, contains only symbols, or has no meaningful text:
1. Default to **English**
2. Inform user of the default
3. Remind user they can override with "reply in [language]"

### Override

User can change the detected language at any time by saying:
- "reply in Japanese"
- "reply in English"
- "use Japanese for replies"
