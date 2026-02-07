## Product Choice

* **Product:** Telegram
* **Website:** <https://telegram.org>
* **Description:** A cloud-based mobile and desktop messaging app with a focus on security and speed.

## Main components

![Component Diagram](./diagrams/out/telegram/component-diagram/Component%20Diagram.svg)

[Link to PlantUML Code](./diagrams/src/telegram/component-diagram.puml)

**Components description:**

1. **Mobile/Desktop Client:** The user-facing application installed on devices (iOS, Android, Desktop) that handles local storage, encryption, and UI rendering.
2. **MTProto Proxy/Load Balancer:** The entry point for all client connections; it manages the encrypted connection and routes requests to the appropriate backend services.
3. **Auth Service:** Handles user registration, login verification via SMS codes, and session management.
4. **Message Database:** A distributed database system that stores chat history, user metadata, and contact lists for cloud chats.
5. **Media Storage / CDN:** A distributed file storage system responsible for uploading, storing, and delivering images, videos, and documents efficiently.

## Data flow

![Sequence Diagram](./diagrams/out/telegram/sequence-diagram/Sequence%20Diagram.svg)

[Link to PlantUML Code](./diagrams/src/telegram/sequence-diagram.puml)

**Flow description:**

* **Selected Group:** Sending a Text Message.
* **Description:** The user types a message in the Client App. The Client encrypts the payload using MTProto and sends it to the Server. The Server authenticates the session, saves the message to the Message Database, and then pushes a notification to the Recipient's device via the Notification Service.

## Deployment

![Deployment Diagram](./diagrams/out/telegram/deployment-diagram/Deployment%20Diagram.svg)

[Link to PlantUML Code](./diagrams/src/telegram/deployment-diagram.puml)

**Description:**
The client applications are deployed on end-user devices (smartphones, tablets, PCs). The backend infrastructure is deployed across multiple geographically distributed Data Centers (DCs). This setup ensures low latency by connecting users to the nearest DC and provides redundancy.

## Assumptions

1. I assume the Notification Service relies on third-party providers like Apple APNs and Google FCM to wake up mobile devices when the app is in the background.
2. I assume that "Cloud Chats" use client-server encryption where the server holds the keys, whereas "Secret Chats" use end-to-end encryption where keys never leave the devices.

## Open questions

1. How exactly is the "global search" implemented across such a massive distributed database of public channels and users?
2. What specific logic is used in the spam filtering component to detect bot networks without compromising user privacy?
