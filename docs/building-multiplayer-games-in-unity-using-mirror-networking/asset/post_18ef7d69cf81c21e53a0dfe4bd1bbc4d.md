
I studied computer science to learn more.

I got into Unity development when it was around version 4. Not worrying about renderer, animations, physics, multiplatform, etc. was nice. I kept at it, making some small games.

Meanwhile, I tried Java, Python, Clojure, Erlang, and probably ten other languages for the MMO server. I learned a lot, especially with Erlang. Still, the situation seemed not ideal.

On summer vacation in August 2015, I saw the UNET beta. I tried it; it was full of bugs, but server and client in one project, SyncVars, Rpc s, etc. It blew me away. I knew that this was the solution to the problem. It was super obvious.

The first thing I tried was their moving ball demo. I immediately started on uMMORPG.

I created health bars, monsters, NPCs, etc. UNET bugs were frustrating, but I released the first version in December 2015, hoping to make enough money to buy a pizza while developing sometimes.

It turns out many people wanted to make an MMO, which finally allowed me to work on it full time.

UNET is still full of bugs. I begged pretty much everyone on the team for fixes – nothing.

So, I ended up forking it to fix some of them myself, back when we still had to replace Unity.Networking.dll in the Unity installation folder. Weaver wasn’t even open source at first.

That’s how Mirror started.

It’s 2021 now, been a while – thousands of bug fixes and improvements. The goal is to finish the roadmap this year, then maybe take a vacation and finally start working on my MMO.

Some people already have small MMOs running with Mirror today. It seems like it’s finally possible.

I still want to remake the same MMO I played when I was 15. Maybe it’s too late now, and people don’t enjoy grinders anymore. But it doesn’t matter.

I just have to do it for myself.

At least networking is open source now.
That matters a lot.
Someday, we’ll have an open-source engine too.

## Do You Have Any Other Advice That You’d Like to Include for My Readers?

- Eat healthily, don’t do drugs, and hit the gym.
- Coding 80+ hours a week is hard.
- Train your body so it can keep up with your mind.

## Do You Want to Thank Anyone?

- Unity. I love this engine. I really do.
- Sean Riley and Alexey Abramychеv from the UNET team for making this possible.
- Paul Pacheco for his work on Mirror and pushing for tests.
- All the contributors.
- I want to thank everyone who helped out in the Mirror Discord.
- And the Unity Asset Store team for helping me to get my dream job.

## Curt – Senior Solutions Architect

Curt is a senior solutions architect and a true industry veteran with 9 years under his belt.

## Tell Me a Bit About Yourself and Where You Come From

I got into game services by administering Counter-Strike servers in the early 2000s.
From there, I applied for roles in the game industry, off the back of this, starting with managing servers for CS LAN tournaments.

I then worked my way to more robust roles, focusing on supporting online games and managing integrations of enterprise-level games with orchestration systems.

My skills grew to include matchmakers and related services within developer needs.

Now, I help to construct the end-to-end solutions of those backend services, leaning on my previous experiences.

## What Are Some of the Biggest Networking-Related Challenges That You and Your Team, or Clients, Have Faced While Shipping Multiplayer Games?

Netcode. When I work with developers, the first challenge is always netcode. Every game is different, and as such, there are no perfect out-of-the-box solutions unless you are making an arena shooter.

The next is backend services asking questions such as the following:

- What dedicated server would you use? Unity headless server?
- What netcode/transport would you use: Photon Bolt, Unity MLAPI, or custom?
- How many players max would the game-server support? 30 or 3000?
- How would you allocate a game server from dynamic capacity, or would you not scale at all?
- What would make the allocation? A Matchmaker? Another service? How would you reconcile in use with available services and game servers? How would you manage these services? And importantly, who would support them?

Figure out your target network tolerances (RTT), 50/100ms/200ms, etc., and determine if the net code supports such latencies.

Decide how many clusters you would want to support worldwide.

How would you direct players to regional services; can they change regions manually?

How does the player flow (the loop, play game, repeat) look like in terms of backend services?

How is it secure?

## How Much Should You Be Spending on Cloud Infrastructure as a Percentage of Your Budget?

It depends on the importance; is it online only?

## What Is Your Preferred Cloud Infrastructure Provider for Databases?

Azure for MySQL and GCP or AWS for other databases.

## What Is Your Preferred Cloud Infrastructure Provider for Server Hosting?

Unity Multiplay

## What Is the Most Crucial Piece of Advice You Could Give to Someone Learning Multiplayer Game Development?

The unknown questions are the most important.

Position yourself as the player as much as possible to find them. Users with bad network connectivity on their end will blame you for bad servers, as an example.

Make sure your game is enjoyable above all else.

## CHAPTER 8

## Challenge What You Know

The best way to learn is through practice. As such, I’ve curated some challenges for you to practice what you’ve learned throughout this book. These challenges will be split into three categories, and I’d encourage you to complete them in order.

Beginner challenges will tackle the basic intricacies of multiplayer development using Mirror Networking. Then, we’ll move onto intermediate challenges, expanding on the previous challenges with user data persistence. Finally, we’ll wrap up by extending the challenges further – adding world and user data persistence.

I encourage you to leverage AWS or Azure’s free-tier servers to complete these challenges – this will help foster your experience with these platforms and be extremely valuable in your career.

I will lay out these challenges in a format that describes the challenge, the required conditions to complete the challenges, and finally, some bonus conditions if you’d like to challenge yourself further.

And remember to take a break, and don’t burn yourself out.

Let’s get started.

## Beginner Challenges – Core Multiplayer Functionality

I will focus the following challenges on fostering your ability to create core, multiplayer functionality that will help relay game state between clients. You don’t need to make these challenges pretty, so please use primitive shapes and objects if no art is available. We’re focused on the multiplayer here. If you’d like to make it look a bit more polished, that’s great too, but don’t lose any sleep over it.

## The Challenge

For your challenge, I want you to recreate the game in Unity using Mirror Networking. Your game should meet the following criteria:

- Have the main menu that allows the player to do the following:
- Start a new game, displaying their current IP address and game server port beneath the play area. The player starting the game should become the host.
- Join a game, allowing the player to enter the IP address and port of another player’s game server. The game should display a timeout message and return to the main menu if the player could not connect.
- Exit the application.
- A round-based system that performs the following:
- A round begins after a delay of 5 seconds.
- Spawn a ball at the center that fires off in a random direction toward one of the players. IE. Not directly upward. Consider clamping the starting angles.
- Each player should have a paddle that they alone can control along the vertical axis of the screen.
- You should clamp each player’s paddle to prevent it from moving out of the play area.
- The ball should reflect off of the top and bottom walls.
- The ball should reflect off of the player’s paddle, with an adjusted angle based on how close it was to the center of the paddle.
- The ball should gain speed when colliding with a player’s paddle.
- You should reset the ball speed at the start of each round.
- If the ball hits the left wall, add a score point to the second player and start a new round. Inversely, if the ball hits the right wall, add a score point to the first player.
- A round should end once a player reaches 10 points.
