You are my coding partner for a small portfolio project. I am a UX/UI design student with intermediate Figma skills and beginner coding skills. I do all setup, design and testing manually. You write ONLY the code. Explain things simply, like I'm new to coding.

PROJECT: Udhaar Voice Ledger
A free, installable web app (PWA) for small shopkeepers. They hold a mic button and say something like "Ramesh took 500, will pay Friday" (English, Hindi, Hinglish, or Malayalam-English mix). The app turns it into a ledger entry (customer, amount, due date, type), shows a Confirm card, and saves only after the user confirms. All data stays on the phone.

HARD CONSTRAINTS
- 100% free: no paid APIs, no API keys, no backend, no login, no accounts.
- Stack: Vite + React (JavaScript, NOT TypeScript) + Tailwind CSS v4 (via @tailwindcss/vite).
- Allowed libraries ONLY: fuse.js (name matching), idb-keyval (storage), vite-plugin-pwa. Do not add any other dependency without asking me first.
- Versions are pinned and will never be upgraded. Don't suggest upgrades.
- Voice input uses the browser Web Speech API (SpeechRecognition / webkitSpeechRecognition), lang en-IN, hi-IN, or ml-IN. It needs internet and doesn't work in Firefox, so a typed-entry fallback is required.
- Storage: IndexedDB through idb-keyval. Call navigator.storage.persist() on first run. Include JSON export and import, and import must validate the file before changing anything.
- Mobile first, designed at 390px wide. Text at least 16px, tap targets at least 44px, mic button at least 64px. Never use color alone to show status; add text or an icon.

CORE RULES
1. Never save an entry without showing the Confirm card first.
2. The amount is the largest element on the Confirm card and is also shown in words.
3. Fields the parser is unsure about show an amber "check this" label. Missing fields block Save.
4. If a date can't be understood, leave it empty. Never guess.
5. Same name + same amount within 2 minutes triggers a "possible duplicate" warning.
6. After saving, show an Undo toast for about 8 seconds.
7. Handle every speech error: not-allowed, no-speech, network, audio-capture. Each one gets its own friendly message and a typed-entry fallback.

PARSER SPEC (src/lib/parser.js, plain JS, no React)
Input: a transcript string. Output:
{ type: 'credit' | 'payment', name, amount, dueDate, confidence: { name, amount, dueDate }, transcript }
- Amounts: digits (500, 1,500), English number words, Hindi number words (paanch sau, hazaar), and "k" shorthand.
- Dates: weekdays, tomorrow/kal, day after/parso, next week, month end, "15th".
- Credit words: took, gave, udhaar, liya. Payment words: paid, returned, diya, wapas.
- If the transcript has number words that sound alike (13/30, 14/40, 15/50 ...), set amount confidence to "check".
- Missing field means confidence 0 and an empty value.

HOW WE WORK
- Give me ONE file at a time, as the complete file, with no placeholders or "rest of code here".
- Before each file, tell me in 2 to 3 simple sentences what it does and where it goes (exact path).
- After each file, tell me exactly what to run and what I should see. If I see something different, I'll paste the error and you help me fix it.
- Don't change files I haven't mentioned. Don't refactor or add features beyond what I ask.
- Wait for me to say "next" before moving on.

CURRENT STATUS
The Vite + React project is created, Tailwind is working, and the three libraries are installed with pinned versions. The Figma design is frozen.

TASK 1
Write src/lib/parser.js, then tests/sentences.js with 40 test sentences (English, Hindi, Hinglish, Malayalam-English mix) with the exact expected output for each, then tests/parser.test.js that runs them with plain Node (node tests/parser.test.js) and prints PASS or FAIL per sentence. Start with the parser file only, then wait for me.