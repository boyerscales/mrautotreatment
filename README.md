# Mr. Auto Treatment website (preview)

One self-contained `index.html`, no build step. Hosted on GitHub Pages.

Everything tweakable is in the `CFG` object near the bottom of the file. Anything
marked `PLACEHOLDER` is still a guess.

## What the owner has confirmed
- Phone: **(916) 613-6405**
- Hours: **6am to 6pm, every day** ("I typically do 6am to 6pm")
- **Stage 1 — Essential Supreme Shine**, $150 for a sedan, shown as "from $150".
  His maintenance package.
- **Executive Supreme Shine** — "I do everything": headlight restoration, under
  carriage cleaning, full interior, full exterior, shampoo seats and carpets,
  shampoo headliner, steam clean seats/carpets/AC vents, window tinting, oil
  change, curb rash correction.

He never mentioned paint correction or ceramic coating, so both were removed.
The site used to sell them.

## Still open
1. **Price for the Executive package.** It currently reads "By quote" and the
   card's button goes to the quote form instead of the calendar. Give it a
   number and set `from:` on the `executive` service plus `book:true`, and it
   joins the calendar on its own.
2. Prices for the à-la-carte jobs (tint, oil change, curb rash, headlights).
   They're listed without prices under "Also on its own".
3. Drop-off address, travel radius and the $1.50/mile rate.
4. Lead time (`LEAD_HOURS`, currently 24) and how long Executive really takes
   (`minutes`, currently 480).
5. Logo at `assets/logo.png`. The text wordmark shows until it exists.

## Going live checklist
1. Real prices in `SERVICES`.
2. Drop-off spot, radius and travel fee.
3. `PREVIEW: false` removes the ribbon.
4. Remove `<meta name="robots" content="noindex">`.
5. Create the dashboard account so the calendar saves (see below).

## Booking
The page asks `https://dashboard.boyerscales.com/api/public/schedule/mrautotreatment`.
While that slug doesn't exist (404), the calendar runs in preview mode: fake busy
slots, nothing saved, and the confirmation says so. As soon as the client account
is created with slug `mrautotreatment`, bookings save for real with no redeploy.

`booking_config.services` ids must match `SERVICES` here. Right now that's
**`essential`** only, since `executive` is quote-only. Add `executive` there too
if you flip it to `book:true`.

The booking POST sends `vehicle_size` (Sedan / SUV / Truck) as its own field and
also prepends it to `notes`, so it survives a backend that doesn't know the field.

## Quote form
No backend yet. It opens a text to (916) 613-6405 with the details filled in so
the customer can attach photos. "Get a Price" on the Executive card and the link
in the booking panel both land here with that option already ticked.

## Photos
Missing files just drop out of the grid. Expected names in `assets/`:
`chevy-c10.jpg`, `headlight.jpg`, `charger.jpg`, `porsche-tall.jpg`,
`odyssey.jpg`, `porsche-foam.jpg`. Three of those are missing today.

## Local preview
```
python3 -m http.server 4899
```
