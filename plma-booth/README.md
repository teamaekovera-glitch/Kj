# PLMA Booth Copilot

A phone-first page for capturing supplier conversations at PLMA 2026, published as a private claude.ai artifact:
https://claude.ai/artifact/4kSKYfSkxd4NnJgAXdJzoK

It runs inside claude.ai and uses the signed-in person's own connectors (Fireflies, Gmail) and Claude. There are no API keys and nothing to deploy.

## What happens at the booth

1. **Record in Fireflies** (mobile app). Open with "This is <name> from <company>."
2. **Capture tab:** snap the business card (Claude reads name, title, company, email, phone, website), take a photo together, and optionally hand over the phone for the one-line **Founding Supplier** sign-up with a finger signature.
3. **Save supplier.** The page then:
   - matches the newest booth recording by time (recordings with no meeting link, from the show start date on);
   - pulls MOQ, certifications, Big 9 allergens, categories, packaging, ingredients, location, capacity, lead time and expertise from the transcript, and shows them as a "Facility Facts" panel;
   - renames the Fireflies recording to `PLMA 2026 · Company · Contact`;
   - emails Varun right away with the facts, the contact details and the business card attached, asking for the facility preview link;
   - drafts that supplier's Thursday follow-up email.

Recordings that never got a card become suppliers on their own after 15 minutes (can be turned off in Setup).

## The rest of the week

| When | What | How |
|---|---|---|
| Wed Nov 18, 7:52 AM CT | Varun gets one reminder listing every supplier still missing a preview link | Scheduled task `trig_01Sw41hyVuYHf6eqzCVD3orM`, or **Remind Varun** on the Thursday tab |
| Thu Nov 19, 7:47 AM CT | Follow-ups go out from Gmail, Varun in CC, preview link filled in. Suppliers without a preview go to Varun to send himself | Scheduled task `trig_01HJssX2SiHAwhzbUigdGPWh`, or **Send all ready** on the Thursday tab |

The two scheduled tasks were created without a Gmail connector, because the tool that created them can't attach one. Add Gmail to each in claude.ai's scheduled tasks (routines) settings, or use the Thursday-tab buttons.

## Data (artifact database)

- `config/settings`: names, Varun's email, event, show start date, sign-up wording, auto-send and auto-create switches, dismissed recordings.
- `leads/<id>`: contact fields, `cardAssetId` / `selfieAssetId` / `signature`, `fireflies`, `details`, `varun.{sentAt,threadId}`, `previewUrl`, `followup.{subject,body,status}`.
- `claims/<recordingId>`: stops two phones from attaching the same recording to two suppliers.
