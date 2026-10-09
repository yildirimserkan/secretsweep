# SecretSweep

Offline, browser-based secret & sensitive data scanner for source code. Single HTML file, no install, no server, no telemetry. Your code never leaves the browser.

> 🇹🇷 Kaynak kodda secret ve hassas veri arayan, tamamen tarayıcıda çalışan tarayıcı. Tek HTML dosyası; kurulum, sunucu veya veri gönderimi yok. Türkçe özet aşağıda.

## Features

- **Any language, any size** — scans pasted snippets, single files or whole folders (drag & drop)
- **Non-blocking** — scanning runs in a Web Worker with streamed, chunked file reading; the UI stays responsive on 100 MB+ files
- **Four detection layers**
  1. Known formats: AWS, GitHub, GitLab, Slack, Stripe, Google, OpenAI, Anthropic, SendGrid, npm, Telegram, Azure Storage, JWT, private keys, credentials in URLs and connection strings
  2. `key = "value"` assignments (JS, Python, Java, C#, Go, PHP, YAML, JSON, .env, properties, XML)
  3. Shannon entropy for unknown-format strings (optional)
  4. False-positive reduction (see below)
- **Label-aware** — `password="password"`, `passwordLabel = "Şifre"`, `pwdHint = "Enter your password"`, i18n keys and `PASSWORD_REGEX` are classified as **INFO**, not as secrets
- **Weak values kept visible** — `changeme`, `admin123`, `letmein` are reported as **LOW** (possible default/hardcoded credentials), never silently dropped
- **References ignored** — `process.env.X`, `os.getenv()`, `${VAR}`, `{{ secrets.X }}`, Vault/SSM references
- **Masked by default** — values and exports are masked unless you explicitly reveal them
- **Export** — copy findings as JSON or CSV; copy the rule set as `rules.json`
- **Skips noise** — `node_modules`, `.git`, `vendor`, `dist`, binaries, lock files, minified files and source maps (toggleable)

## Usage

1. Download `index.html` and open it in a browser, **or** enable GitHub Pages for this repo
2. Paste code (Ctrl+Enter to scan), or switch to **Dosya / Klasör** and drop a file or folder
3. Toggle severities with the chips; INFO is hidden by default

## Severity

| Level | Meaning |
|---|---|
| CRITICAL | Known high-impact format (cloud keys, PATs, private keys) |
| HIGH | Likely real secret (assignment with non-trivial value, credentials in URLs) |
| MEDIUM | High-entropy string or JWT |
| LOW | Placeholder or weak value — may still be a default credential |
| INFO | Label, UI text, i18n key or metadata field |

Paths matching `test/`, `mock/`, `fixtures/`, `example/` or `*.spec.*` are downgraded one level (toggleable).

## Inline ignore

Add any of these to a line to suppress findings on it:

```
secretsweep:ignore
gitleaks:allow
pragma: allowlist secret
```

## Limitations

- Does not scan git history (use `gitleaks` / `trufflehog` for that)
- Does not validate whether a key is live
- Multi-word passphrases may be classified as natural-language text (INFO)
- Private keys are flagged on the `BEGIN` line only

## Türkçe özet

- `index.html` dosyasını indirip tarayıcıda aç veya GitHub Pages ile yayınla.
- Kod yapıştır ya da dosya/klasör sürükle-bırak. Kod hiçbir yere gönderilmez.
- `password="password"` gibi label tanımları **INFO** olarak işaretlenir ve varsayılan olarak gizlidir.
- `changeme` / `admin123` gibi zayıf değerler **LOW** olarak raporlanır, çünkü default credential olabilirler.
- Değerler varsayılan olarak maskelidir, dışa aktarımlar da öyle.

## License

MIT © 2026 Serkan Yıldırım
