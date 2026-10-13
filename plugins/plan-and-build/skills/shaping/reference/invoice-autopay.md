# Invoice Autopay

Read this when you work through a user flow. The case shows how a few places, affordances, and
connections can bring up questions about behavior before anyone draws a screen.

A breadboard describes an interface in words, without any visual design. It has three parts:

- **Places** are things a person can go to, such as screens, dialogs, or menus.
- **Affordances** are things a person can act on, such as buttons and fields. Text on the screen
  counts too, because reading it tells the person what happened or what they can do next.
- **Connections** show how an affordance takes the person from one place to another.

In the drawings below, each place is a name with a line under it, its affordances are listed under
the line, and an arrow is a connection.

## Start from a concrete entry point

An invoicing product is considering a feature that pays a customer's future invoices
automatically. The team started from a customer who is looking at an unpaid invoice, and one
candidate puts a "Turn on Autopay" button there:

```text
Invoice                Setup Autopay            Confirmation
───────                ─────────────            ────────────
Turn on Autopay ─────▶ Card fields              Thank-you message
                       Institution logo
                       Submit ────────────────▶
```

Whether Setup Autopay is a separate page or a dialog is still open, because the breadboard only
shows what connects to what.

## Let the flow show the missing decision

Walking through the candidate raised a question the team hadn't thought of. Turning on Autopay
might pay the current invoice, or it might only arrange payment of future invoices, and a thank-you
message can't settle behavior the flow hasn't defined.

One option added a "Pay balance now" choice to the setup screen. The option added another decision,
and the confirmation screen would then have to show a receipt in some cases and only confirm the
setup in others.

The team tried a different entry point instead, and made Autopay an option in the existing payment
flow:

```text
Invoice        Pay Invoice                   Confirmation
───────        ───────────                   ────────────
Pay ─────────▶ Payment details               Receipt
               Autopay future invoices?      Thank-you message
               Submit ─────────────────────▶ Autopay confirmed, if chosen
```

In the new flow, the customer always pays the current invoice, so the question goes away. Sketching
the flow also reminded the team that the existing payment form accepted ACH bank transfers as well
as credit cards, and they confirmed that Autopay could use either one. The payment methods were a
decision for this product, and other products may need different ones.

## Follow the interaction until you find where it stops

Turning Autopay on isn't the whole experience, because a customer also needs a way to turn it off
later. Many customers had no username or password, because they paid each invoice through its own
unique link. A self-service page would have needed a new account system, which would have added a
lot of work. For this project, the team decided that a customer could ask the invoicer to turn
Autopay off, and they added one action to a page that invoicers already had:

```text
Customer list          Customer detail
─────────────          ───────────────
Customer detail ─────▶ Disable Autopay
```

The solution reused an existing place instead of adding a login and a page for managing Autopay.
Whether that compromise works depends on the product and its customers, and the case doesn't set a
general rule against self-service.

The result was two small elements that connect to each other: an Autopay option on the existing
payment screen, and a disable action for the invoicer. Visual design and ordinary implementation
choices stayed open, while the behavior for the current invoice and the limit on who can turn
Autopay off were explicit.

Source: [Find the Elements](https://basecamp.com/shapeup/1.3-chapter-04).
