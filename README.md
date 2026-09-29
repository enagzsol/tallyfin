# Amazon Seller – home screen clone

Pixel-measured clone of the Amazon Seller app home screen (UK marketplace) at iPhone 15/16 size
(393 x 852 pt, 3x). Every position, colour and font size was measured from the screenshots in
`screenshots/` and verified against them with a headless-browser overlay.

## Run

Serve the folder over HTTP (the fonts need a real origin):

    python3 -m http.server 8765

Then open http://localhost:8765 on your Mac, or http://<your-mac-ip>:8765 on your iPhone and add it
to the Home Screen for a full-screen version with the real status bar.

## How it works

- Opening the app shows the navy launch screen with the wordmark for about two seconds, then fades to the dashboard.
- On every launch a sheet asks for today's figures: total sales, order items, total balance and the
  feedback numbers. Optional fields: total sales for the last 7 days, last 30 days, this month, this
  year and last year, the Orders card counts, buyer messages, IPI, deal and voucher sales.
  Tap the "amazon seller" logo or the ⋮ on the Sales card to open it again.
- All figures are stored in the browser and drive every timeframe consistently: today's total is the
  last bar of 7D, WEEK, 30D and MONTH and part of this month's bar in YEAR; the 7-day total contains
  today; the 30-day total contains the 7 days, and so on. Periods you leave blank are estimated from
  the ones you entered.
- Sales are spread across hours, days and months with a human-looking pattern (night lull, morning
  ramp, evening peak, weekday/weekend and seasonal differences) plus seeded noise, so the shape is
  irregular but stable: re-entering a total only rescales that period, it never reshuffles the bars.
- Dates, axis labels and comparison labels ("yesterday", "last 7D", "last week", "last 30D", previous
  month name, previous year) are computed from the current date and time.

## Daily backlog and simulation

- Every day you enter is stored by date. Open the sheet (logo or the Sales card's three dots), pick a
  day at the top, and enter or overwrite its figures. Days already shown are never reshuffled.
- The ALL dropdown on the Orders card opens the **Simulation** switch. When it is on, each new day is
  generated from your recent level and trend with weekday and seasonal patterns, plus events:
  stock-outs (several days near zero, then a catch-up), supply hiccups, cash-flow squeezes,
  promotions and one-day dips. Today's figure grows through the day. Days that have passed are frozen
  into the backlog; the balance accrues net sales with a fortnightly payout and feedback counts tick up
  with orders.
- Switch it off to enter or overwrite days yourself; the entry sheet also opens on launch while it is
  off. Switch it back on and the simulation continues from what you entered (later simulated days are
  regenerated from the new trend).

## Query parameters (for testing)

- `?range=d7|week|d30|month|year` – open on that timeframe
- `?state=none` – don't open the entry sheet; `?state=setup` – open it immediately
- `?demo=1` – sample figures, nothing saved; `?demo=sim&ago=20` – as if entered 20 days ago and simulated since
- `?splash=0` – skip the launch screen (any number = how long it holds, in ms)

## Layout

- `index.html`, `css/styles.css`, `js/app.js` – the app
- `fonts/modern/` – Ember Modern Text and Ember Modern Display (Amazon's 2025 typefaces by NaN, the
  ones the app really uses; Regular and Bold, fetched from Amazon's own web CDN). The app's medium
  weight is not published, so "Your store is healthy", the tabs and "View reports" use Regular with a
  hairline stroke. `fonts/` also holds classic Amazon Ember (unused) and SF Pro for the status bar
  and tab bar.
- `assets/` – logo, flag, icons and status-bar glyphs cropped from the original screenshots
- `screenshots/` – the original screenshots
