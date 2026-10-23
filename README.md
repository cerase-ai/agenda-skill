# agenda-skill

A Cerase skill that has the assistant read the person's calendar and list the day's or the week's appointments, with time, attendees, place or video link, the material each carries and any overlap.

## What the assistant does

1. **The agenda**: reads the calendar on the organisation's clock and writes one message, each appointment with its time, attendees, place or video link and material, and any overlap.
2. **Preparing a meeting**: offers to prepare one of the meetings.
3. **A call**: for a call with a video link, offers to send the meeting capture, booking it only for the call the person chooses.
4. **Every morning**: asked to receive the agenda daily, it asks only the time and schedules one recurring message, which waits for the person's approval.

## What it never does

- Books or records a call the person did not choose.
- Changes anything in the calendar.

## Requirements

The calendar (`google-workspace`), and the meetings connector (`cerase-meetings`) for the capture.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The instructions the assistant loads. |
| `cerase.json` | Marketplace manifest: namespace `studio.guidance`, name `agenda`, display name, description, licence. |
| `i18n.yaml` | Italian display name and description for the Marketplace; not sent to the assistant. |
| `LICENSE` | MIT licence text. |
| `SECURITY.md` | How to report a vulnerability. |
