# Mr. Auto Treatment website (preview)

One self-contained `index.html`, no build step. Hosted on GitHub Pages.

Everything tweakable is in the `CFG` object near the bottom of the file. Anything
marked `PLACEHOLDER` is a guess until the owner confirms it.

## Going live checklist
1. Real phone number: `PHONE_E164`, `PHONE_TEXT`, and set `PHONE_IS_PLACEHOLDER: false`.
2. Real prices, times and add-ons in `SERVICES` and `ADDONS`.
3. Drop-off spot, hours, radius and travel fee.
4. `PREVIEW: false` removes the ribbon.
5. Remove `<meta name="robots" content="noindex">`.
6. Logo at `assets/logo.png` (the text wordmark shows until it exists).

## Booking
The page asks `https://dashboard.boyerscales.com/api/public/schedule/mrautotreatment`.
While that slug doesn't exist (404), the calendar runs in preview mode: fake busy
slots, nothing saved, and the confirmation says so. As soon as the client account is
created with slug `mrautotreatment`, bookings save for real with no redeploy.
`booking_config.services` ids must be `full`, `interior`, `exterior`.

## Photos
Missing files just drop out of the grid. Expected names in `assets/`:
`chevy-c10.jpg`, `headlight.jpg`, `charger.jpg`, `porsche-tall.jpg`, `odyssey.jpg`, `porsche-foam.jpg`.

## Quote form
No backend yet. With a real number set, it opens a text to the business with the
details filled in so the customer can attach photos.
