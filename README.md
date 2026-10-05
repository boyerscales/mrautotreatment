# Mr. Auto Treatment website

One self-contained `index.html`, no build step. Hosted on GitHub Pages.

Everything tweakable is in the `CFG` object near the bottom of the file. Anything
marked `PLACEHOLDER` is still a guess.

> **Rebuilt 2026-10-01 on the Valley Details system.** Same type scale, 2px buttons,
> thin rules, numbered lists, swipe row and motion (hero settle and scroll fade,
> clip-reveal photos with drift, staggered reveals, receding work cards, a phone bar
> that hides near the hero and the booking form). Gold where VD uses red. Black and
> gold kept, as he asked. No gradients on type or buttons.

## Where things came from
- **Logo:** his Instagram profile picture (@mrautotreatment_), 545px. `assets/logo.jpg`,
  plus `logo-180.png` (home-screen icon) and `favicon.png`. It's on the header, hero
  seal, about, booking panel, booking summary, quote form, reviews, finale and footer.
- **Photos:** cover frames of his Instagram posts. Full-size originals live in
  `assets/originals/` (gitignored). Web copies were cut with
  `sips -s format jpeg -s formatOptions 68 -Z 1100`. Every shot is a portrait phone
  photo, which is why the hero is a three-photo strip rather than one wide image.
- **Tagline, claims, payment types, service area:** his own car-door magnet
  (Instagram post DdIu3LXhNsq): "Elite Detailing, where professionalism meets
  perfection", licensed & insured, satisfaction guaranteed, premium products,
  monthly plans, fleet accounts, RV/boat/motorcycle work, cash/card/Zelle/Apple
  Pay/Google Pay, business invoices, mrautotreatment@gmail.com.

## What he confirmed (text, 2026-10-01)
| Package | Old name | Sedan | Crossover | SUV | Truck / Van | Time |
|---|---|---|---|---|---|---|
| Executive Bronze Detail (`essential`) | Stage 1, was Essential Supreme Shine | $150 | $175 | $200 | $200+ | ~2 hr |
| Elite Platinum Detail (`platinum`) | Stage 3, was Executive Supreme Shine | $300 | $325 | $350 | $350+ | ~3 hr |

- Only **two** packages: **Stage 1** = Executive Bronze, **Stage 3** = Elite Platinum.
  He confirmed there is no Stage 2. The site labels them "Stage 1 · Bronze" and
  "Stage 3 · Platinum".
- Every price is shown as **starting at**. Bigger vehicles (family vans, single cab
  trucks) cost extra, so Truck / Van shows a `+` and the copy says he texts the number.
- Platinum contents are his words: pre-rinse, contact wash, wheels and wells, tire
  dressing, paint decontamination, 6-month paint protection, deep vacuum, dash and
  console disinfected and treated, cloth and leather seats refreshed.
- Bronze bullets: he said "everything you have in stage 1 is correct, I'll double
  check". Treat as confirmed unless he comes back.
- Travel: free within about 30 minutes, 45 min to an hour out costs extra.
- **$25 deposit** to lock in a spot (no-shows and last-minute reschedules).

## Booking
Live against `https://dashboard.boyerscales.com/api/public/schedule/mrautotreatment`.
Real bookings save and real texts go out, so **test with the network stubbed**, never
by submitting the form.

**Run `Dashboards/Client Dash/mrautotreatment-packages-2026-10.sql` once.** Until then
the dashboard only knows `essential` under its old name, and a Platinum booking is
saved with the default 120-minute length instead of 180.

The dashboard stores one price per service, so the booking POST also puts
`SUV · from $350 · $25 deposit to collect` in `notes`, plus `vehicle_size`.

## Deposit
**The site follows the dashboard (2026-10-05).** The schedule API reports `deposit`: 0
until his Stripe is connected in the dashboard, then 25. At 0 the booking box says
"Due today: Nothing" / "Book my spot", the How It Works step reads "Get a text", and the
deposit FAQ is hidden. At 25 everything flips back on its own and a booking goes straight
to Stripe checkout. Nothing to edit here when he connects.

`CFG.DEPOSIT.URL` is empty, so right now the confirmation says *"We'll text you a
secure link to pay it."* **Someone has to actually send that link** until the URL is set.

To finish it:
1. He makes a Stripe account in his own name (money goes straight to him).
2. Payment Links → $25 "Booking deposit". After payment, redirect to
   `https://<his site>/?deposit=paid#book`, which shows a "Deposit received" toast.
3. Paste the link into `CFG.DEPOSIT.URL`. The confirmation becomes a "Pay $25
   Deposit" button, and `client_reference_id` carries `MRA-<date><time>-<last 4 of phone>`
   so he can match payments to bookings in Stripe.

Moving money through BoyerScales instead (Stripe Connect) works, but it's dashboard
backend work: onboarding, webhooks, and a booking that only confirms once paid.

`CFG.DEPOSIT.POLICY` ("goes toward your total, carries over with 24 hours' notice,
no-shows lose it") is **our wording, not his**. Confirm it with him before launch.

## Still open
1. His **About paragraph** (he's sending it). Paste into `CFG.ABOUT`.
2. Deposit policy wording, and the Stripe link above.
3. Travel fee amount for 45–60 min out, and where he's based.
4. Reviews: `CFG.REVIEWS` is empty, so the section shows a "Leave a review" card.
   He has a Reviews highlight on Instagram. Add real ones as `{text, name, car}`.
   Never write them ourselves.
5. OK to show "Emmanuel" as the owner name? (`CFG.OWNER_FIRST`, '' hides it)
6. Licensed & insured is from his magnet. Worth a quick yes from him since it's a claim.

## Going live checklist
1. He pays us.
2. ~~Domain → GitHub Pages, add `CNAME`.~~ Done 2026-10-05. **mrautotreatment.com** is
   in Elijah's Namecheap account: four A records on `@` (185.199.108–111.153) and
   `www` CNAME → `boyerscales.github.io.`
3. ~~Remove `<meta name="robots" content="noindex">`.~~ Done 2026-10-05.
4. Point the Stripe redirect at the real domain (`siteUrl` in his `booking_config`).
5. Turn on Enforce HTTPS in the repo's Pages settings once GitHub issues the certificate.

## Local preview
```
python3 -m http.server 4899
```
