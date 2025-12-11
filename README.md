# 🚀 High-Performance Real-Time Gaming Leaderboard

[![Node.js](https://img.shields.io/badge/Node.js-18.x-339933?style=for-the-badge&logo=node.js)](https://nodejs.org/)
[![Redis](https://img.shields.io/badge/Redis-ZSET-DC382D?style=for-the-badge&logo=redis&logoColor=white)](https://redis.io/)
[![Socket.io](https://img.shields.io/badge/Socket.io-RealTime-010101?style=for-the-badge&logo=socket.io)](https://socket.io/)
[![React](https://img.shields.io/badge/Frontend-React-61DAFB?style=for-the-badge&logo=react)](https://reactjs.org/)

A highly scalable leaderboard system designed to handle **10,000+ concurrent users** with sub-millisecond latency. Unlike traditional SQL-based leaderboards that choke under load, this system utilizes **Redis Sorted Sets (ZSET)** and **WebSockets** for instant updates.

## 🏗️ System Architecture

The following diagram illustrates how user scores flow from the client to the Redis cache in real-time.

```mermaid
sequenceDiagram
    participant User as 🎮 User/Client
    participant Server as ⚡ Node.js Server
    participant Redis as 🔴 Redis Cache (ZSET)
    participant DB as 🗄️ MongoDB (Persistent)

    User->>Server: Emits 'updateScore' (via Socket.io)
    Server->>Redis: ZADD leaderboard <score> <userId>
    Redis-->>Server: Returns New Rank (O(log N))
    Server->>DB: Async Write (Data Persistence)
    Server-->>User: Broadcasts 'LiveRankUpdate'
