---
name: arabic-rtl-mixed-text
description: Write Arabic (or any right-to-left language — Hebrew, Persian, Urdu) replies that stay readable when they contain English terms, code identifiers, file names, numbers, versions or commit hashes. Use whenever replying in Arabic/RTL in a chat or markdown UI and the reply mixes in any Latin text — technical terms like prompt, commit, branch, API, file paths, hashes, ports, model names — or when the user complains that the text "looks reversed", "the words are jumbled", "the brackets are flipped", or "اتجاه الكلام غريب / الكلام مقلوب".
---

# Arabic / RTL text mixed with English — keep it readable

## The problem

Chat and markdown interfaces lay out each line with the Unicode Bidirectional Algorithm. Two of its rules break Arabic replies that contain English:

1. **The first strong letter of a paragraph decides its direction.** A line or list item that *starts* with an English word (`prompt`, `API`, a file name) is laid out left-to-right, so the Arabic that follows is pushed into the wrong order and alignment.
2. **Neutral characters take the direction of their neighbours.** Brackets `( )`, dots, colons, dashes, slashes and digits have no direction of their own. Between an English word and an Arabic word they attach to the wrong side: brackets appear flipped, a full stop jumps to the start of the line, an option letter `أ)` lands after its text.

The user sees something like:

```
prompt لا (أرسلته ب) أ) وكيل الموقع
```

when what was written was:

```
وكيل الموقع: هل أرسلت الـ prompt؟ أ) نعم ب) لا
```

Keep technical terms in English — translating `commit` or `branch` makes things harder to follow. The goal is **placement**, not translation.

## The rules

1. **Wrap every Latin-script item in inline code.** This covers English words, file names, paths, commands, versions, hashes, ports and model names: `prompt`, `d823c731`, `handoff.md`, `localhost:3310`. Most interfaces draw inline code as an isolated box, so it stays one visual unit and doesn't pull the punctuation around it.

2. **Never start or end a line, heading, list item or table cell with Latin text.** Start and end with an Arabic word. If the English item would come first, put an Arabic word in front of it.
   - ✗ prompt وكيل الموقع جاهز
   - ✓ تعليمات وكيل الموقع جاهزة، وهي الـ `prompt` التالي
   - ✗ d823c731 هو آخر إصدار
   - ✓ آخر إصدار هو `d823c731` على الفرع الحالي

3. **No English inside ordinary brackets `( )` in an Arabic sentence.** Use a comma, a dash, guillemets « », or a new sentence instead.
   - ✗ أرسل التعليمات (prompt) للوكيل
   - ✓ أرسل التعليمات للوكيل — الـ `prompt` الذي بالأسفل

4. **Put each choice on its own line, with the question on a line before them.** Never put options inline after mixed text.
   ```
   هل أرسلت الـ `prompt` لوكيل الموقع؟
   أ) نعم، أرسلته
   ب) لا، لم أرسله
   ```

5. **Keep a sentence's punctuation next to Arabic, not next to English.** End sentences with an Arabic word before the full stop or question mark. Don't write «…الفرع main.»; write «…على الفرع `main` حالياً.»

6. **Long English content goes in a fenced code block**, not inside a paragraph. This covers prompts for other agents, commands and URLs. The Arabic explanation goes on the lines above and below.

7. **Numbers with units or symbols:** keep the number and its unit together, and preferably in inline code when the unit is Latin (`5 GB`, `1280×800`). Arabic units (٥ أيام، 30 يوماً) need nothing special.

8. **Tables:** start each cell with Arabic, or make the cell English-only and wrap it in code. Don't put a mixed cell whose first character is Latin.

## Optional: invisible direction marks

If the interface does not isolate inline code and a trailing English word still drags punctuation away, add a RIGHT-TO-LEFT MARK (U+200F) right after the English word, before the punctuation. Use this only in prose, never inside code, commands or anything the user will copy, because the mark is copied too.

## Self-check before sending

Read every line of the reply and ask:

- Does it start with an Arabic letter? Lines that start with a number or bullet are fine if the first *word* is Arabic.
- Does it end with an Arabic word before the final punctuation?
- Is every Latin item inside backticks?
- Are there any `( )` containing Latin?
- Is every choice on its own line?

If any answer is wrong, rewrite that line. Do not translate the term to fix it; move it.

## Before / after

✗ Hard to read once rendered:

```
**وكيل الموقع** (الشريط، والمقاسات، وإكسل ريسا): أ) أرسلته ب) لا prompt
3310 شغالة الآن على آخر commit عند وكيل الموقع (d823c731).
```

✓ Renders in order:

```
**وكيل الموقع** — الشريط والمقاسات وإكسل ريسا.
هل أرسلت له الـ `prompt`؟
أ) نعم، أرسلته
ب) لا، لم أرسله

نسخة التجربة `3310` تعمل الآن على آخر إصدار عند وكيل الموقع، وهو `d823c731`.
```
