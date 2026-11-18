# Doraemon chatbot + WhatsApp: setup, deployment, testing

## What was added
| File | Status | Purpose |
|---|---|---|
| `portfolio-site.html` | modified | Floating WhatsApp + Doraemon buttons, chat panel (built lazily on first open), WhatsApp Click-to-Chat, config keys `whatsappNumber` / `chatEndpoint`. The Supabase contact form code is untouched. |
| `supabase/functions/chat/index.ts` | new | `POST /functions/v1/chat`: validation, rate limit, timeout, system prompt, calls the AI provider with a server-side key. |
| `supabase/functions/chat/knowledge.json` | new | Knowledge base extracted from your portfolio (about, education, experience, skills, 5 projects, contact). Edit it when your portfolio changes. |
| `.env.example`, `.gitignore` | new | Server secret names; keeps real secrets out of Git. |
| `supabase-setup.sql` | from before | Contact form table. |

Your site is a single HTML file, so the chatbot CSS/JS is inline there (same pattern as the rest of the site) instead of separate `chatbot.css` / `chatbot.js` files. The backend is a Supabase Edge Function because a static page cannot hold an AI key safely, and you already use Supabase.

## 1. WhatsApp (configured: 917387625095)
In `PORTFOLIO_CONFIG` set `whatsappNumber` to country code + number, digits only, no `+`, spaces or dashes (India example: `919876543210`). Until then, clicking WhatsApp shows a "not configured" notice and the console warns once. Visitors always review and press send themselves.

Pre-filled messages: general (floating button), project inquiry, collaboration, hiring. The chatbot shows the matching "Discuss on WhatsApp" button based on what the visitor asks.

## 2. Chatbot backend
1. Install the Supabase CLI and log in: `supabase login`, then `supabase link --project-ref uhbfikzqtmemitjilepf`.
2. Copy `.env.example` to `.env`, fill in `ANTHROPIC_API_KEY` (or set `AI_PROVIDER=openai` and `OPENAI_API_KEY`) and `ALLOWED_ORIGINS` (your real site URL, plus `http://localhost:PORT` while testing).
3. `supabase secrets set --env-file ./.env`
4. `supabase functions deploy chat --no-verify-jwt`
   `--no-verify-jwt` is required: the site is public and the new publishable keys are not JWTs. Protection comes from the origin allow-list, input limits, and rate limiting.
5. Reload the site. The endpoint defaults to `<supabaseUrl>/functions/v1/chat`.

Security notes: the AI key exists only as an Edge Function secret. The browser sends only the visitor's message and the last 10 messages. The function does not log message contents. The rate limit (8/min, 60/hour per IP) is per function instance and resets on restart, so it is a good deterrent, not a hard guarantee. For stronger limits, track counts in a Supabase table or put a CAPTCHA in front. Set a monthly spend cap in your AI provider dashboard too.

## 3. Optional: WhatsApp Business Cloud API (not implemented)
Not added, because it needs a Meta Business account, a verified number and credentials you have not provided. The current Click-to-Chat button needs none of that. When you are ready: add a second Edge Function (`whatsapp-webhook`) that answers Meta's `GET` verification using `WHATSAPP_VERIFY_TOKEN`, verifies the `X-Hub-Signature-256` header on `POST`, and sends replies with `WHATSAPP_ACCESS_TOKEN` + `WHATSAPP_PHONE_NUMBER_ID` through the official Graph API, using approved message templates outside the 24-hour window. The placeholder variable names are already in `.env.example`. Do not use unofficial WhatsApp libraries.

## 4. Behaviour summary
- Doraemon opens/closes (button, X, Esc), minimizes, clears the conversation, shows typing dots, blocks empty/duplicate sends, keeps session history in memory, and is full-screen on phones with background scroll locked.
- Error text: "I'm having trouble responding right now. You can contact Sahil directly through email or WhatsApp." (with Email and WhatsApp buttons). I changed "me" to "Sahil" so the assistant does not speak as him.
- Buttons are side by side at the bottom-right (WhatsApp left, Doraemon right) and the panel opens above both, so nothing overlaps. Toasts moved up to sit above them.
- The avatar is a plain "D" monogram. I did not use any Doraemon character artwork, which is copyrighted.

## 5. Testing report (what I ran)
Run in headless Chromium with a stubbed chat endpoint and stubbed Supabase client; the Edge Function was run with a mocked AI provider.

| Test | Result |
|---|---|
| Open / close / Esc / minimize / clear | Pass |
| Quick-question chip sends the question | Pass |
| Multi-message context (history sent with 2nd request) | Pass |
| Empty input | Send disabled, nothing submitted: pass |
| API failure | Friendly error + Email/WhatsApp buttons: pass |
| Collaboration / project / contact intents | Correct action buttons and WhatsApp messages: pass |
| WhatsApp link format | `https://wa.me/<digits>?text=<encoded>`: pass |
| Missing WhatsApp number | Notice shown, no broken link: pass |
| Buttons / panel overlap (desktop, 390px mobile) | None: pass |
| Mobile: full-screen, no horizontal scroll, scroll lock restored on close | Pass |
| Existing contact form still inserts | Pass |
| Page JavaScript errors | None |
| Edge Function: empty/too-long/bad JSON/blocked origin/rate limit | 400/400/400/403/429: pass |

Not tested (needs your accounts): a real AI response, answer quality on your real deployment, and opening a real WhatsApp chat. Please try these after deploying, and ask the bot about each project, an unrelated question (it should redirect), and "hire" / "collaborate".
