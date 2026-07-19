# Hisaab — split bills, stay friends

A working prototype of a "Split the Bill" tool for the Indian market, built as a single-file
web app. Live demo:https://github.com/ManjeetKaur94/hisaab.git

**The reframe:** the brief asked for a tool to "figure out who owes whom." In India, the hard
part isn't the arithmetic — it's the *settling*. UPI is universal, WhatsApp is where plans
happen, and yet the category leader (Splitwise) supports neither natively. Hisaab is built
around three moves: equal-by-default splits with a "Pay for others" escape hatch (covering
the uneven-split case socially, not with spreadsheet fields), one-tap UPI settlement via
deep links, and WhatsApp join-links so a group builds itself instead of being typed in.

## Running it
Open `index.html` in any browser — no build step, no dependencies, no backend.
Everything lives in-browser; state persists via localStorage on the deployed site.
Append `?reset` to the URL to restore the seeded demo data.

Test on a phone for the full experience: "Pay Now" fires a real `upi://pay` deep link
(opens GPay/PhonePe/Paytm), and WhatsApp share/invite/remind actions open real `wa.me` links.

## Code structure (single file, by design)
- **Design tokens** — CSS custom properties extracted 1:1 from the Figma file
  (Inter, #222/#717171 text, #EBEBEB hairlines, radius-28 cards, the red/green/orange
  badge system, gradient friend-strips, white pill CTAs).
- **Data model** — `splits[]`: each split has a name, date, total, creator (`by`), and
  `members[]` with `pending/paid` status. Shares are computed (`total / members.length`),
  never stored, so counts stay consistent as people join.
- **Views** — four screens (`home`, `detail`, `amount`, `people`, plus a contacts modal)
  toggled by a tiny router (`go()`); bottom sheets are rendered into one shared layer.
- **State machine on the detail screen** — the bottom CTA derives from role + status:
  receiver-pending → *Pay Now*; receiver-paid → *You've paid*; creator-with-pending →
  *Remind*; creator-all-paid → *Close group*. Selection modes (Pay for others /
  Paid already) overlay radios on pending rows only.
- **No frameworks** — vanilla JS + template strings. For a prototype this size, a framework
  would add a build step without adding capability; every behavior is inspectable in one file.

## Built with AI (Claude), directed by hand
The prototype went through 9 major iterations (three full visual directions were built and
rejected before this one). The AI wrote the code; the design decisions, scope cuts, Figma
source of truth, and every correction — from a broken balance calculation to silently
dropped image assets during a refactor — came from review of its output.

## What I'd do next
Real backend + auth, UPI collect requests (not just intent links), contact-book sync,
receipt OCR for amount autofill, and reminder scheduling.
