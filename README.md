# ChronoScore 🏆

ChronoScore is a high-performance, real-time leaderboard system built with **Java** and **Spring Boot**. It is designed to handle high-concurrency score updates and deliver instantaneous ranking changes to clients using a low-latency architecture powered by **Redis** and **WebSockets**.

## 🚀 Key Features

* **Real-Time Updates**: Instant score propagation using WebSockets (STOMP).
* **High Performance**: Leverages Redis Sorted Sets ($O(\log(N))$ complexity) for lightning-fast ranking and retrieval.
* **Scalable Architecture**: Designed as a stateless microservice ready for containerization.
* **Asynchronous Processing**: Efficient handling of score submissions to ensure minimal API response times.
* **RESTful API**: Clean endpoints for manual score management and user registration.

## 🛠️ Tech Stack

| Component | Technology |
| :--- | :--- |
| **Backend Framework** | Spring Boot 3.x |
| **Data Store** | Redis (Sorted Sets) |
| **Communication** | WebSockets / STOMP |
| **Build Tool** | Maven |
| **Language** | Java 17+ |

## 🏗️ Architecture Overview

ChronoScore utilizes a "Push-over-Pull" model to ensure users see updates without refreshing:

1.  **Ingestion**: Users submit scores via REST or WebSocket.
2.  **Processing**: The application updates the Redis `ZSET`, which automatically re-ranks the user.
3.  **Broadcast**: A WebSocket event is triggered, pushing the updated Top 10 (or relevant window) to all connected subscribers.

## 🚦 Getting Started

### Prerequisites

* JDK 17 or higher
* Maven 3.8+
* Redis Server (Local or Cloud)

### Installation

1. **Clone the repository**
   ```bash
   git clone [https://github.com/raymondoyondi/ChronoScore.git](https://github.com/raymondoyondi/ChronoScore.git)
   cd ChronoScore

2. **Configure Redis**
Update `src/main/resources/application.properties` with your Redis credentials:
   ```
   spring.data.redis.host=localhost
   spring.data.redis.port=6379
   ```

3. **Build and Run**
   ```
   mvn clean install
   mvn spring-boot:run
   ```

## 📡 API Endpoints (Examples)

### REST API
*   **`POST /api/scores`**: Submit a new score.
*   **`GET /api/leaderboard`**: Retrieve the current top rankings.

### WebSocket Topics
*   **`/topic/leaderboard`**: Subscribe here to receive real-time JSON updates of the rank standings.

## 📈 Performance Notes

By utilizing Redis Sorted Sets, the system calculates ranks based on the following:

*   **Insertion/Update:** $O(\log(N))$
*   **Range Retrieval:** $O(\log(N) + M)$ (where $M$ is the number of elements retrieved).

This ensures that whether you have 1,000 or 1,000,000 users, the latency remains minimal.
