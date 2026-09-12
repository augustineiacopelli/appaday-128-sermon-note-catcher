# AppADay 128: Sermon Note Catcher

A single-page tool for capturing notes on a Catholic homily right after Mass. It looks up the day's Mass readings and liturgical celebration automatically, lets you confirm or edit them, then helps you record and clean up your notes on the homily itself.

## What it does

On load, it fetches the current date's readings and liturgical calendar entry from the free Catholic Readings API (cpbjr.github.io/catholic-readings-api). On Saturdays it fetches both Saturday and the following Sunday, and defaults to whichever one fits the time of day (before 4pm suggests the vigil, after suggests tomorrow's Sunday Mass), with a toggle to switch between them without a new network call.

The confirmation screen shows the Mass name, season, and all four reading citations as editable fields, prefilled from the API, plus a deterministic link straight to the official USCCB reading page for that date built from the date alone. If the free API lookup comes back empty, that link becomes the prominent way to check and enter the citations by hand instead of relying on a third party dataset.

The whole interface also recolors itself to the day's actual liturgical color: green for Ordinary Time, violet for Advent and Lent, white and gold for Christmas and Easter, red for martyrs, Pentecost, Palm Sunday, and Good Friday, with rose available for Gaudete and Laetare Sunday. The color name is shown in the header and on both the confirmation and note screens, and a simple cross motif and gold ornamental dividers carry the Catholic visual identity throughout.

On the note screen, a microphone button uses the browser's built-in speech recognition to capture the homily as you listen, writing interim and final results into a raw transcript box. When recognition ends, or when you use the manual "clean up notes" button on browsers without speech support, the raw transcript is parsed into clean paragraphs: filler words dropped, sentences capitalized and punctuated, and paragraph breaks inserted wherever there was a real pause. A quick scan for phrases like "I will" or "the main point is" suggests a takeaway and an action item, each marked "Suggested."

If a Claude API key is entered in Settings and AI enhancement is turned on, the parsed transcript is sent to Claude to refine the takeaway and action item, and those fields are marked "AI refined" instead. If that call fails for any reason, the heuristic suggestions are left in place with no interruption.

Entries save to local storage keyed by the date of the Mass you attended, and the history screen lists every saved entry for review or editing. An email button builds a short mailto message with the Mass name, date, theme, takeaway, and action item, and a separate copy button puts the full parsed transcript on the clipboard for pasting in by hand.

## Build

Single `index.html` file. No build step, no frameworks. Google Fonts (Cormorant Garamond, Inter) via CDN is the only external dependency besides the two data APIs and the optional Claude API call.

## Category

AppADay category S, Spirituality. The Claude integration is an optional enhancement layer rather than the core function, so `ai:true` should still be set on the portal entry since it does call the Claude API when enabled.
