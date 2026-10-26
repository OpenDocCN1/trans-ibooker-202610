
The player should have the ability to walk up to the crop and water it using their watering can, which they start with and cannot sell.

The watering can, can be refilled by using it on water. Using the watering can will consume water from the can and will need to be refilled periodically.

Crops that reach max growth will consume water at a reduced rate, and harvesting gives crops and seeds.

## The Challenge

Let’s break this down into challenge points:

- Your game must include the main menu, which allows the player to exit the game or join a dedicated server.
- There must be an escape menu that allows the player to exit the game.
- Upon joining a dedicated server, allow the player to create an account or log in. The user should choose a username, password, and display name. For the sake of this challenge, you can handle all logins using plaintext with a masked password field.
- During login, you should handle authentication with a custom Mirror Authenticator. The server can then compare the username and password to a DynamoDB database and allow or reject the client.
- You should store all player-related data within the database. Your data storage should include things like:
  - Player position
  - Player hot bar
  - Player money
  - Player account details
- There should be a buying, planting, watering, harvesting, and selling gameplay loop with all actions authoritative to the server.
- The game server should support up to eight players.
- Player display names should appear below the players.
- You should create a small play space for the game that includes:
range of the crow to scare it away, the crow destroys the crop, leaving a dead crop that the player needs to clear.

## Tips

As with the recent challenges, here’s a helping hand:

- You can use scriptable objects to create your item data. Doing so makes it easy to expand the game with new crop types. You can make a class serializable within a scriptable object by giving the class the [Serializable] attribute.
- Camera.ScreenToWorldPoint is a great way to get the player’s mouse position in world space.
- To calculate consumption rates, you can use “remaining -= waterConsumptionRatePerSecond * Time.deltaTime” to reduce the remaining water relatively accurately by using that formula within the Update loop.
- Remember to sanitize any user inputs as they could mess with your database or game server.
- Do not save the plant and world state to the database every tick as this could burn through your rate limits on AWS DynamoDB. Instead, consider saving critical information like player trades immediately and saving world state every few minutes in as few database calls as possible.
- The saving and loading will likely be one of the most significant challenges of this exercise, and that’s the point – don’t give up.
- Consider using MessagePackC# for Unity, linked in challenge #3, to aid with your saving and loading of world data.
- A player should be able to harvest and water another player’s crop; that’s something to keep in mind when planning out your authority structure.

## Challenge #5 – Box-Hoarder

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

- Create a basic login and account creation process when starting the game. The user should be able to store a username, password, and display name. We’re not too worried about security for this challenge.
- Store information about a user, such as the number of crates delivered successfully, their playtime, and how many boxes they have stolen from an enemy demarcated area.
- Create a menu option called “Safe House” when the player joins the safe house, loads up a scene, and spawns in boxes for each box the player has delivered during play. The game should store this data within their account. When leaving the safe house, save the location of all boxes for the next time the player joins the safe house.
- Add sound effects for crates hitting objects, such as the floor or other crates.
- Add some ambient music to the game.
- Allow players to be stunned and knocked down if a delivery box hits them that another player threw.
- Add a minimap that allows players to see the layout of the map and an overview of control point ownership. This minimap should not show the location of players or crates.
- Allow the minimap to show how many crates are within each demarcated area.
- Allow the minimap to show how many crates are currently active.
- Allow the player to choose an untaken color, and save that as a preference to their account.
- Add the ability for players to jump.
- Add a more vertical map design that allows players to throw crates down several floors. Players do not take fall damage.
- Allow a player to carry up to two delivery crates at a time.

## Tips

The following tips may prove helpful:

- You can use network authentication to send different custom requests to the server, such as querying information about your account. That way, you can build a custom authentication enum that states you intend to query information without joining. The server can see this request, validate your login, query the data, return the data, and reject your connection.
- Make use of Network Rigidbody.
- Don’t be afraid to skip some of the bonus challenges. They may prove very challenging.
- If you’re stuck on something, move on to a different problem, and revisit your issue afterward.

If you were able to complete these challenges, then you’re well on your way to building multiplayer games in Unity using Mirror Networking. If you got stuck, then I’d encourage you to revisit parts of the book.

Multiplayer code is tricky, and sadly, there is no MakeMultiplayer() method yet.

The biggest challenge is getting into the mindset of multiplayer code. It requires a different way of thinking about challenges that, traditionally, you might find easy. There are many things to consider.

All in all, the best way to improve once you have a good foundation is through practice.

As such, I encourage you to seek out new challenges and come up with your challenges too; with that said, the most crucial part is to start small. If you come up with a monumental challenge that burns you out after a month, then there’s no point.

There are many game idea generators out there. I encourage you to use them to come up with your challenges. Good luck!
