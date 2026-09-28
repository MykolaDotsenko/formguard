# FormGuard — Accessible Validation Lab

[![Quality](https://github.com/MykolaDotsenko/formguard/actions/workflows/quality.yml/badge.svg)](https://github.com/MykolaDotsenko/formguard/actions/workflows/quality.yml)

**A four-field registration form used to explore validation, progressive enhancement and accessible error recovery without a framework.**

[**Open FormGuard →**](https://mykoladotsenko.github.io/formguard/) · [Architecture](./ARCHITECTURE.md)

Nothing is submitted, stored or tracked.

## Interaction flow

1. enter username, email and password;
2. touched fields receive contextual feedback;
3. invalid submit focuses the first problem;
4. valid submit moves focus to a success state;
5. reset returns the form to a clean state.

## Validation stays outside the DOM

```text
HTML / CSS
    ↑
script.js
browser + interaction adapter
    ↓
src/validation.js
pure validation rules
```

The validation module knows nothing about `window`, `document`, CSS classes, storage or network requests.

The browser layer owns touched state, attributes, focus and password visibility.

## Progressive enhancement

Native HTML constraints remain in the document.

Custom validation is enabled only after all required DOM references have been resolved. If JavaScript initialization fails, the form falls back to native browser validation instead of ending in a half-enhanced state.

## Validation policy

- Unicode-aware normalized username;
- pragmatic email shape check;
- password length/letter/number/no-whitespace rules;
- password cannot contain normalized username or email local part;
- strength is advisory and separate from validity.

Client-side validation is UX, not a security boundary. A real backend would validate again and own password hashing, verification, sessions and rate limiting.

## Accessibility

- labels and contextual descriptions;
- dedicated live error regions;
- synchronized `aria-invalid`;
- first-error focus recovery;
- success/reset focus management;
- visible keyboard focus;
- reduced-motion support;
- responsive touch targets.

## Stack

- semantic HTML
- modern CSS
- Vanilla JavaScript / native modules
- Node built-in tests
- Playwright
- axe-core
- Lighthouse CI
- GitHub Actions

Zero runtime dependencies.

## Quality

```bash
npm ci
npm run check
npm run test:e2e:deps
npx playwright install chromium firefox
npm run test:e2e
```

The suite covers boundary rules, touched-field timing, dependent password rules, focus behaviour, success/reset, responsive presentation and accessibility states.

## Run locally

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000`.

## License

MIT.
