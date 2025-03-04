---
title: Chess.com
---

## Requirements
### Functional
1. User signin/signup
2. Must be able to choose a game and play 
   1. 3-min 
   2. 5-min 
   3. 10-min
3. The system must match players based on the type of the game chosen and rating of the player.
4. Time is kept during the game. Once the match is over, player rating must be adjusted
5. Users must not be able to cheat by modifying time.
   
### Scale
1.    100 million users
2.    10 million active users(last 90 days active)
3.    100k users playing games at a time

## Design
![HLD](../../assets/images/chess.com.drawio.svg)

### Matching service
The crux is the designing of the matching service. 
The matching service needs to be performant(user shouldn't have to wait long in case there is a match available). 
With 100k matches going on at a time, we can assume a few 10’s of thousands of play requests per sec.

The matching service also should be correct. The same player should not be assigned to two matches at the same time.

Using a database to store the match requests and then trying to find matches is tricky from a correctness perspective. 
Will have to use locks(select for update) on match requests which might not be performant.

By using streaming constructs, we can pipe matchable requests into the same window as illustrated in the diagram. 
Since the execution within a window is single-threaded matches can be found safely and elegantly.

![streaming-matching](../../assets/images/streaming-matching.svg)

There will be some windows(rating 700–2000) which will have more requests than the others. 
The matching logic is simple since all users in a window can be matched against each other. 
So this should be performant.

### User management service
Nothing much to talk about. The usual stuff. Some redundancy for fault tolerance. 
DB can be a relational store with replication and partitioning if required.

### Gateway and load balancer
To route and load balance http requests to the matching and user management service

### Game service
This is the service that coordinates the game between two players selected by the matching service. 
It also enforces the rules of the game via a chess engine. 
The chess engine is not the focus of this design.

The communication between the clients and the service is bidirectional. 
The client communicates with the server when the user makes a move. 
The server then communicates with the client of the opposing player to propagate this move and so on.

This pattern of communications lends itself to the use of websockets.

The game service maintains all information about the game in a db. 
The predominant operation is a write of moves. 
So a db designed for fast writes like cassandra(leaderless writes) can be used.  
The table can be partitioned by game id so that write parallelism is at the level of each game.

Leaderless writes implies write-conflict resolution.
This is not a concern in this use case due to the partitioning strategy(game-id). 
We will not have conflicting writes.

### Assignment service
Once the match between two players is decided, the assignment service takes over. 
It assigns a particular instance of the game service to the clients of the matched players.

Once this is done, the game service opens two web-socket connections with the player clients for game play.

The assignment service maintains this mapping information(game-id → instance-id) in case connection is broken between 
the game service instance and the clients. 
Either one of them can connect with the assignment service to open a new web-socket connection to continue.

The assignment service uses a consistent hash-based assignment strategy. 
This helps in minimum migration of clients to a different game server instance. 
Migration of clients is required because
1. An existing instance may go down
2. Existing instances may become overloaded and a new one might need to be introduced to reduce load

A coordination service like zookeeper is used to monitor the game service instances and store the mapping information.

## Todo
* Consistent hash based assignment strategy

