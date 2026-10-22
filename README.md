# Smart Real Estate — Egypt landing + AI valuation (static)

Arabic-first static site: landing, listings, details, sell, auth pages, admin dashboard, wishlist, AI price estimate. Served by `nginx:alpine` with hardened config. No backend.

## Quickstart

```bash
docker build -t smart-real-estate .
docker run -p 8080:80 smart-real-estate
# open http://localhost:8080
```

Or serve statically: `npx serve .`.

## Layout

```
index.html, home.html, all-properties.html, details.html
sell.html, login.html, signup.html, admin.html, evaluate.html, wishlist.html
assets/css/  # global, auth, home, landing
assets/js/scripts.js  # filters + Gemini valuation hook
Dockerfile            # nginx:alpine + nginx-hardened.conf
```

## AI valuation — security warning

`assets/js/scripts.js:60` has `const apiKey = ''` wired to `generativelanguage.googleapis.com...?key=${apiKey}` **in the browser**. Empty by default (safe), but never paste a real key there — it ships to every visitor.

Production path: put Gemini behind a tiny backend proxy (Cloud Run / Vercel function) that holds the key server-side, rate-limits, and returns only the estimate. Frontend then calls `/api/valuate` with no key.

## License

MIT — see [LICENSE](LICENSE).
