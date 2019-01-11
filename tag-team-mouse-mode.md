# Tag Team

## Overview

This design document describes a proposed new mouse mode for the Tuple client.

In contrast to Free-for-all Mode (where the host and guest each have their own
pointer), Tag Team lets a host and a guest trade off controlling a single mouse
pointer by clicking once to take control.


## Context

As of v0.11.5 (1/11/19), Free-for-all Mode is the source of many bugs. 

The differences in external mice vs trackpads and across distinct macOS
versions leads to lots of complexity. It's tricky to handle edge cases like
showing the correct pointer type.

It's also not clear how useful our users find Free-for-all Mode. (Let's show
this doc to them and see what they think.)

We suspect that Tag Team will be much easier to implement correctly.


## Goals

- Provide a mouse mode that offers many of the benefits of Free-for-all Mode,
  but with lower complexity and tendency for buginess.

- Give our early users a reliable mouse mode quickly, which buys time to
  perfect Free-for-all Mode.


## Non-goals

- Unless user feedback strongly suggests that it would be acceptable, the goal
  of Tag Team is not to replace Free-for-all Mode.

- It is not our goal to spend a huge amount of development effort on this task.
  If Tag Team proves to be equally complex to implement, the effort should be
  reevaluated.


## Proposed functionality

Assume two Tuplers start a call and activate Tag Team.

1. The host always starts with control of the mouse.

2. When the host has control, the guest's mouse movements do not affect the
host's mouse pointer.

3. When the host has control, the guest's mouse movements DO affect their local
pointer as usual.

4. When the guest clicks within the Tuple window, control is given to them.

5. When control is given to the guest, the pointer on the host machine is
warped to the current location of the guest's pointer.

6. While the guest has control, their pointer movements affect the cursor on
the host's machine.

7. While the guest has control, the host's mouse movements do not affect the
pointer.

8. Whenever control changes hands, the red border on the screen flashes once.

9. When either side clicks to take control, the click does not fire a click
event on the host machine.


## Open questions

- How much do our users value Free-for-all Mode? Are they using it a lot?

- Should the host should be able to activate Tag Team? I can picture thinking
  "get that extra pointer out of here!" and wanting to change to Tag Team.
  Agreed?


## Milestones

1. Quick-and-dirty proof of concept to evaluate UX.
2. Production-ready implementation deployed to staging for internal testing.
3. Deployed to production and announced to users.
