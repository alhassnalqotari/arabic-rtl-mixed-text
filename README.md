# arabic-rtl-mixed-text

A Claude skill that keeps Arabic (and other right-to-left) replies readable when they contain English terms, file names, numbers, versions or commit hashes.

مهارة لـ Claude تجعل الردود العربية مقروءة عندما تحتوي على مصطلحات إنجليزية أو أسماء ملفات أو أرقام إصدارات.

## The problem / المشكلة

Mixed Arabic + English text in chat interfaces often renders out of order:

- Brackets appear flipped.
- Punctuation jumps to the wrong end of the line.
- Option letters such as `أ)` and `ب)` land after their text.
- A line that starts with an English word gets laid out left-to-right.

The cause is the Unicode Bidirectional Algorithm, not the language model. The fix is placement: the English terms stay as they are, written in a way the renderer can't scramble.

عند خلط العربي والإنجليزي في نفس السطر:

- تنقلب الأقواس.
- تقفز علامات الترقيم إلى الطرف الخطأ من السطر.
- يتلخبط ترتيب الخيارات.

الحل ليس ترجمة المصطلحات، بل وضعها بطريقة لا تقلب الجملة.

## Install / التثبيت

**Claude Code** — copy the folder into your skills directory:

```bash
# for one project
mkdir -p .claude/skills && cp -r arabic-rtl-mixed-text .claude/skills/
# for every project
mkdir -p ~/.claude/skills && cp -r arabic-rtl-mixed-text ~/.claude/skills/
```

**Claude apps (claude.ai / desktop)** — zip the folder and upload it under *Settings → Capabilities → Skills*. Or install the `.skill` file from the releases.

## What it does / ماذا تفعل

The skill tells Claude to:

1. Wrap every Latin-script item in inline code.
2. Never start or end a line, list item or table cell with English.
3. Never put English inside ordinary brackets in an Arabic sentence.
4. Put each choice on its own line, with the question above.
5. Put long English content, such as prompts and commands, in fenced code blocks.
6. Check every line before sending.

See [`SKILL.md`](SKILL.md) for the full rules and before/after examples.

## License

MIT
