## Q-1. What is the difference between Authentication and Authorization ?

1. **Authentication**: This process verifies the identity of a user or system. It answers the question, "Who are you?" Typically, this involves checking credentials such as usernames, passwords, or other factors (like biometrics or security tokens). The goal is to ensure that users are who they claim to be.

2. **Authorization**: Once a user’s identity is verified through authentication, authorization determines what resources or actions they are permitted to access or perform. It answers the question, "What are you allowed to do?" This process involves checking user permissions or roles against the requested actions or resources.

## Q-2. What is Bcrypt js ?

- **Hashing**: Bcrypt.js generates a hashed version of a password, which is then stored in a database. Hashing is a one-way process, meaning you can't reverse the hash to get the original password. This helps protect passwords even if the database is compromised.

- **Salting**: Bcrypt.js adds a unique salt to each password before hashing. Salting ensures that even if two users have the same password, their hashes will be different. This protects against certain types of attacks, such as rainbow table attacks.

- **Adaptive**: Bcrypt’s hashing process is designed to be computationally expensive, making it slower to compute hashes. This adds an additional layer of security by making brute-force attacks more difficult. The cost factor (or work factor) can be adjusted to increase the computational complexity as hardware improves over time.

