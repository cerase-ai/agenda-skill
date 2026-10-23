---
name: agenda
description: "Use when a scheduled message asks for the day's agenda, when the person asks what is in their calendar today, tomorrow or this week, or when they ask to receive their agenda every morning, to change its time or to stop it. It reads the calendar on the organisation's clock and writes one message: each appointment with its time, who attends, its place or video link and the material it carries, and any overlap. It offers to prepare one of the meetings and, for a call with a video link, to send the meeting capture to it. Reading the calendar changes nothing in it; the capture is booked only for the call the person chooses, and to receive the agenda every morning it asks only the time, then schedules one recurring message on working days that names this skill, which waits for the person's approval."
---

# Agenda

You tell the person what is in their calendar for the day or the week they ask about: each appointment, when, with whom, where, and what it needs. One message, read in a minute.

## Turning it on

When the person asks to receive the agenda every morning:

1. If they did not say at what time, ask the time and nothing else, in one short question. It runs on working days, Monday to Friday, unless they name other days.
2. Read the scheduled messages: `call_recipe("cerase-schedule.list_schedules", {})`. If one already names this skill, change its time with `call_recipe("cerase-schedule.update_schedule", {"id": "<its id>", "cron_expression": "<minute> <hour> * * 1-5"})` instead of creating a second one.
3. Otherwise schedule it: `call_recipe("cerase-schedule.create_recurring", {"label": "Agenda del giorno", "message_body": "Prepara l'agenda di oggi con la competenza agenda.", "cron_expression": "<minute> <hour> * * 1-5"})`, with the label and the message in the person's language when it is not Italian. The message names this skill and repeats none of its steps. The call waits for the person's approval, and that approval is their confirmation: do not also ask for it in the chat.
4. While the approval waits, say only that the agenda starts once the person approves. When the note with its result arrives, say in one line when the first agenda comes, in the words of the result's `next_run`: never work the day or the time out yourself.

To stop it, cancel that message with `call_recipe("cerase-schedule.cancel_schedule", {"id": "<its id>"})` once the person asks.

## Reading the calendar

1. The window, on the organisation's clock: today, or the day or the week the person names. `call_recipe("google-workspace.getCalendarEvents", {"timeMin": "<the window's start, RFC3339 with the organisation's offset>", "timeMax": "<its end>"})`.
2. An event whose description is cut, or that links documents, is read in full: `call_recipe("google-workspace.getCalendarEvent", {"eventId": "<id>"})`.
3. Write the agenda.

Without the calendar among your connectors, or when the call fails, say so in one line and never fill it from memory.

## The agenda

In the person's language. The first line gives the day and the date, or for a week its first and last day.

- **Each appointment**, in order of time, grouped by day for a week: when it starts and ends, the title, who attends and who has not answered, the place or the video link, and the material it carries (its agenda, the documents it links). An all-day event comes first in its day.
- **Overlaps**: an event that overlaps another is said, with both titles.
- **A free day** is said in one line.

## What it offers

End with at most two offers, one line each:

- to prepare one of the meetings, which the meeting preparation skill does;
- for a call with a Google Meet or Microsoft Teams link, when the meetings connector is among yours, to send the meeting capture to it. Nothing is booked before the person says which call. Once they do, book that event: `call_recipe("cerase-meetings.book_meeting_from_link", {"url": "<the event's video link>", "title": "<the event's title>", "starts_at": "<its start>", "ends_at": "<its end>"})`, and repeat what its answer says.

## Hard rules

- The agenda only reads. Nothing is created, moved, accepted or declined in the calendar, and no capture is booked unless the person chooses the call.
- Never invent an appointment, a time, an attendee or a document: what you list is what the calls returned.
- Times are on the organisation's clock, the `now` of the message header.
