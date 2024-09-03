## Q-1. What is the difference between Authentication and Authorization ?

1. **Authentication**: This process verifies the identity of a user or system. It answers the question, "Who are you?" Typically, this involves checking credentials such as usernames, passwords, or other factors (like biometrics or security tokens). The goal is to ensure that users are who they claim to be.

2. **Authorization**: Once a user’s identity is verified through authentication, authorization determines what resources or actions they are permitted to access or perform. It answers the question, "What are you allowed to do?" This process involves checking user permissions or roles against the requested actions or resources.

## Q-2. What is Bcrypt js ?

- **Hashing**: Bcrypt.js generates a hashed version of a password, which is then stored in a database. Hashing is a one-way process, meaning you can't reverse the hash to get the original password. This helps protect passwords even if the database is compromised.

- **Salting**: Bcrypt.js adds a unique salt to each password before hashing. Salting ensures that even if two users have the same password, their hashes will be different. This protects against certain types of attacks, such as rainbow table attacks.

- **Adaptive**: Bcrypt’s hashing process is designed to be computationally expensive, making it slower to compute hashes. This adds an additional layer of security by making brute-force attacks more difficult. The cost factor (or work factor) can be adjusted to increase the computational complexity as hardware improves over time.

## Q-3. What is the main purpose of  Bcrypt js ?

1. **Password Security**: Bcrypt.js converts plaintext passwords into hashed versions, which are stored in the database instead of the actual passwords. This adds a layer of security in case of a data breach.

2. **Salting**: It adds a unique salt to each password before hashing, which helps prevent attacks like rainbow table attacks where precomputed hash values are used to crack passwords.

3. **Adaptive Hashing**: Bcrypt’s hashing algorithm is designed to be slow and computationally intensive, making it more resistant to brute-force attacks. The complexity can be adjusted to ensure that as computing power increases, the hashing process remains secure.

4. **Compatibility**: Bcrypt.js is a JavaScript implementation of the bcrypt algorithm, making it suitable for use in JavaScript environments like Node.js for handling password hashing and verification.

## Q-4. How to use Bcrypt js ?

To use Bcrypt.js in a Node.js application, follow these steps:

1. **Install Bcrypt.js**:
   First, you need to install Bcrypt.js using npm. Open your terminal and run:
   ```bash
   npm install bcryptjs
   ```

2. **Import Bcrypt.js**:
   In your Node.js code, require Bcrypt.js:
   ```javascript
   const bcrypt = require('bcryptjs');
   ```

3. **Hash a Password**:
   Use Bcrypt.js to hash a password. You can specify the number of salt rounds (complexity) as the second argument to the `hash` function. More rounds mean more security but also more computation time.
   ```javascript
   const password = 'yourPasswordHere';
   const saltRounds = 10;

   bcrypt.hash(password, saltRounds, function(err, hash) {
     if (err) {
       console.error('Error hashing password:', err);
       return;
     }
     console.log('Hashed password:', hash);
     // Store the hash in your database
   });
   ```

4. **Compare a Password**:
   To verify a password, use the `compare` function. This checks if a plaintext password matches the hashed password.
   ```javascript
   const hashedPassword = 'storedHashedPasswordHere';
   const enteredPassword = 'userEnteredPassword';

   bcrypt.compare(enteredPassword, hashedPassword, function(err, result) {
     if (err) {
       console.error('Error comparing passwords:', err);
       return;
     }
     if (result) {
       console.log('Password matches!');
     } else {
       console.log('Password does not match.');
     }
   });
   ```

5. **Error Handling**:
   Always handle errors in your callback functions, as shown in the examples. This helps you debug issues and ensures your application handles errors gracefully.

Here’s a complete example of how you might use Bcrypt.js in a simple script:

```javascript
const bcrypt = require('bcryptjs');

const password = 'mySecurePassword';
const saltRounds = 10;

// Hash the password
bcrypt.hash(password, saltRounds, function(err, hash) {
  if (err) {
    console.error('Error hashing password:', err);
    return;
  }

  console.log('Hashed password:', hash);

  // Compare the password
  bcrypt.compare(password, hash, function(err, result) {
    if (err) {
      console.error('Error comparing passwords:', err);
      return;
    }

    if (result) {
      console.log('Password matches!');
    } else {
      console.log('Password does not match.');
    }
  });
});
```


