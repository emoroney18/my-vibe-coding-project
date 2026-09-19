# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: [name your flow]

### Step 1: Expand, build new screens in a strict sequence
```
Search empty state: What happens if no cleaners match the search?
Provider unavailable state: What happens if Maya has no availability for the selected date?
Booking error state: What happens if the booking request fails?
Provider detail/verification interaction: The trust panel shows the proof, but there could be a more detailed view explaining what each verification means.
Back/edit booking state: The current flow moves forward cleanly, but there is no explicit edit/back experience before requesting the booking.
```

### Step 2: Behavior, hard-code the states
```
Loading: What happens between selecting a provider and loading the profile?
Empty: What happens when no providers or dates are available?
Error: What happens if the booking request cannot be completed?
```

### Step 3: Refine, one surgical polish
```
I would refine the trust panel on Maya's profile, because that is explicitly your experiment variable. Your README says the panel contains four verification proofs and that the control/treatment toggle is what makes the experiment testable.
```

## Reusable techniques learned

- Using ChatGPT and keeping it in plan mode help to refine the prompt before waisting credits on Lovable.

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

Nothing broke on the first iteration, but I did refine the experiment evidence section to include a control vs trust panel and to be able to look at the date by date range. 
