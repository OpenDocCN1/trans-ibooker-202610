# Challenge #5 – Box-Hoarder

I’ve designed the following challenge to push your usage of physics within a multiplayer environment, a requirement that many modern multiplayer games have, along with reinforcing your world persistence experience from the last challenge.

This challenge aims to create a first-person game supporting 16 players connecting to a dedicated server. The objective is to collect boxes within the game world and bring them to your claimed delivery point.

There are eight delivery points scattered around the play area. A delivery point contains a demarcated area, a Deliver button, and an indicator that displays the current owner.

The claim indicator is color-coded and shows the percentage of a player’s claim on the delivery point.

If a player stands within the claim indicator, they begin claiming the delivery site. If another player controls a delivery site, they will receive an alert notifying them that a player is attempting to control the delivery site. If two or more players are within the claim indicator, the claiming process pauses until only a single player is within the ring.

Players can pick up a single box at a time, and upon throwing the box, it travels in front of them in a parabolic arc. A player needs to place boxes within a demarcated delivery point that they own and press the deliver button. For each additional crate within the demarcated area, the game multiplies their score for the given delivery. Let’s take a look at the Figure 8-6 for a breakdown of scoring.

| Crates | Score |
|---|---|
| 1 | 1 |
| 2 | 4 |
| 3 | 9 |
| 4 | 12 |
| 5 | 25 |
| 6 | 36 |
| 7 | 49 |

Figure 8-6. The figure depicts the scoring table for the box-hoarder