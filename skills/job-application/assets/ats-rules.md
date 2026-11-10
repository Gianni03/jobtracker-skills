# ATS-Safe Text Rules

These rules are a precaution, not a hard requirement of every ATS. Modern parsers handle standard bullets, hyphens, and most punctuation. The goal is clean text and predictable parsing across old and new systems.

## Keep

- `|` as the section separator.
- Numerals with symbols: "5+ years", "30%", "100+", "90%". Do NOT spell out quantities ("5 plus years", "30 percent").
- Standard bullet characters. `make_docx.py` renders `- ` lines as bullets by default; pass `--plain` for plain indented paragraphs.
- Hyphens in compound words and dates: "end-to-end", "cross-functional", "August 2025 - Present". "August 2025 to Present" is also fine and reads cleanly.
- Exact technology names that contain symbols: `C#`, `C++`, `.NET`, `F#`. Do NOT rewrite them; ATS keyword matching looks for the exact term.

## Avoid or replace

| Symbol | Why | Replacement | Example |
|---|---|---|---|
| `#` in prose | Can confuse legacy parsers and markdown-aware tools | Remove or rephrase | "issue #123" -> "issue 123" |
| `*` | Can be read as a wildcard or markup | Remove or rephrase | "5* rating" -> "5 star rating" |
| `/` in prose | Can merge words in some parsers | Space or "and" | "UI/UX" -> "UI UX", "HTML/CSS" -> "HTML and CSS" |
| Decorative symbols | Noise in the text layer | Remove | Emoji, arrows, box-drawing characters |

## Markdown source vs output

The markdown structural markers (`#`, `##`, `###`, `-`) are fine in the `.md` source. The `make_docx.py` converter turns them into formatting. These rules apply to the TEXT CONTENT and the docx/pdf output only.

## Verification

After generating the PDF, check for the symbols that still matter. `%`, `+`, `-`, and `/` inside URLs are allowed, so they are not in the regex. Technology names like `C#` and `F#` are also allowed.

```bash
pdftotext "<output.pdf>" - | grep -nE '(^|[^A-Za-z])[#*]'
```

Review manually if the output contains `/` outside URLs. Empty output means clean.
