The player should have the ability to walk up to the crop and water it using their watering can, which they start with and cannot sell.

The watering can, can be refilled by using it on water. Using the watering can will consume water from the can and will need to be refilled periodically.

Crops that reach max growth will consume water at a reduced rate, and harvesting gives crops and seeds.

# The Challenge

Let’s break this down into challenge points:

+   - Your game must include the main menu, which allows the player to exit the game or join a dedicated server.
+   - There must be an escape menu that allows the player to exit the game.
+   - Upon joining a dedicated server, allow the player to create an account or log in. The user should choose a username, password, and display name. For the sake of this challenge, you can handle all logins using plaintext with a masked password field.
+   - During login, you should handle authentication with a custom Mirror Authenticator. The server can then compare the username and password to a DynamoDB database and allow or reject the client.
+   - You should store all player-related data within the database. Your data storage should include things like:
+     - Player position
+     - Player hot bar
+     - Player money
+     - Player account details
+   - There should be a buying, planting, watering, harvesting, and selling gameplay loop with all actions authoritative to the server.
+   - The game server should support up to eight players.
+   - Player display names should appear below the players.
+   - You should create a small play space for the game that includes: