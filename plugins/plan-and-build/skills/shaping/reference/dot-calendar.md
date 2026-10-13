# The Dot Grid calendar

Read this when a broad feature request needs narrowing, or when you're deciding how much visual
detail a shape needs.

## Find the situation behind the feature name

Basecamp 3 launched with a schedule that listed events one after another, without a calendar grid.
Customers asked for a calendar, but the word "calendar" covers many possible features, and a full
calendar could take six months or more to build. Earlier versions of Basecamp had a calendar, and
only about 10% of customers used it.

The team called one customer who had asked for a calendar. Instead of asking what the calendar
should look like, they asked what she had been doing when she wanted one. Her office tracked
meeting room bookings on a calendar drawn on a wall. One day, while she worked from home, a client
asked her to schedule a meeting, so she drove to the office to check the wall, and when she got
there she found no free time that suited the client. She needed to see free time from home, and she
didn't need every feature of a calendar.

The existing schedule showed events but not the gaps between them, so it worked for deadlines but
not for booking rooms. The narrowed need gave the team a specific baseline to judge solutions
against.

## Fit the elements to the appetite

In the book, the team was willing to spend one six-week cycle on an improvement, but not six months
on a full calendar. The six weeks were the appetite for this example, and they aren't a default time
budget for other projects. In this skill, find out how much change the user is willing to take on,
and use a time budget when the user gives one.

The resulting concept had three connected elements:

- A read-only grid that shows two months side by side.
- A dot for each event on a day, so the empty days stand out.
- A list of events below the grid. Selecting a day with dots scrolls that day's events into view.

The Dot Grid left out dragging events between days, color coding, and categories. An event that
lasts several days repeats its dot on each day, instead of stretching across the cells as a bar.
The grid is read-only and always shows two months, so there's also no resizing of events and no
daily or weekly view. The team accepted these trade-offs because they understood the use case, and
the cuts kept the work within six weeks.

## Show how the parts relate, and leave the design unfinished

Where the grid and the list sit on the page was part of the idea, so the team drew a fat marker
sketch, which uses strokes too broad to hold fine detail. The sketch showed two month grids side by
side, dots on some days, and rough lines for the list below. It didn't choose type, spacing,
colors, or finished controls. See the [original rough sketch](https://basecamp.com/assets/images/books/shapeup/1.1/calendar_sketch.png).
A text version of the sketch looks like this:

```text
┌─────────────────────┐   ┌─────────────────────┐
│ •   •       • •     │   │       •         •   │
│         •           │   │   •       • • •     │
│   •           •     │   │             •       │
└─────────────────────┘   └─────────────────────┘
 ────────────────────────────────────
 ──────────────────────────
 ───────────────────────────────
```

The sketch was specific about how the feature works and clearly unfinished about how it looks.
Designers could work out the visual design, and programmers could choose the implementation, and
neither had to work out what kind of calendar the project was meant to be.

Use the case to ask which smaller set of elements would improve the actual situation. Don't assume
that every calendar request is about free time, or that the same cuts fit another product. The
customer's story justified these trade-offs, and the name of the feature alone wouldn't have.

Sources: [Principles of Shaping](https://basecamp.com/shapeup/1.1-chapter-02),
[Set Boundaries](https://basecamp.com/shapeup/1.2-chapter-03), and
[Find the Elements](https://basecamp.com/shapeup/1.3-chapter-04).
