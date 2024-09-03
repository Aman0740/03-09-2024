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
## Q-5. One to One relationship in MongoDB ?

In MongoDB, a one-to-one relationship between documents can be implemented in a few ways, depending on the specific use case and requirements. Unlike relational databases, MongoDB does not enforce relationships by default, so the schema design is more flexible. Here are the common approaches for modeling a one-to-one relationship in MongoDB:

### 1. Embedded Documents

In this approach, related data is embedded directly within the parent document. This is suitable when the related data is typically accessed together with the parent document.

**Example:**

Consider a scenario where each user has one profile.

```json
{
  "_id": ObjectId("605c72f7c68b9a253820c57a"),
  "username": "john_doe",
  "email": "john@example.com",
  "profile": {
    "age": 30,
    "address": "123 Main St",
    "phone": "123-456-7890"
  }
}
```

Here, the `profile` is embedded directly in the `user` document, representing a one-to-one relationship.

### 2. Referenced Documents

In this approach, documents reference each other using unique identifiers. This is suitable when the related data is large, frequently updated, or accessed independently.

**Example:**

We can create two collections: `users` and `profiles`, where each `user` references one `profile`.

**`users` collection:**
```json
{
  "_id": ObjectId("605c72f7c68b9a253820c57a"),
  "username": "john_doe",
  "email": "john@example.com",
  "profile_id": ObjectId("605c72f7c68b9a253820c57b")  // Reference to a profile document
}
```

**`profiles` collection:**
```json
{
  "_id": ObjectId("605c72f7c68b9a253820c57b"),
  "age": 30,
  "address": "123 Main St",
  "phone": "123-456-7890"
}
```

Here, the `user` document has a `profile_id` field that references the `_id` of a `profile` document, establishing a one-to-one relationship.

### 3. Hybrid Approach

In some cases, a hybrid approach might be useful, where you embed some small fields in the parent document and use references for larger or more frequently changing data.

### Choosing the Right Approach

- **Embedded Documents**: Best when related data is always retrieved together and is not too large.
- **Referenced Documents**: Best when related data is large, frequently updated, or accessed independently.
- **Hybrid Approach**: Useful when some fields are small and others are large or change frequently.


## Q-6. One to Many relationship in MongoDB ?

In MongoDB, a one-to-many relationship represents a scenario where one document is associated with multiple documents in another collection. MongoDB does not enforce relationships like relational databases, but you can model these relationships using either embedded documents or references. Here’s how you can handle one-to-many relationships:

### 1. **Using Embedded Documents**

In this approach, you embed multiple documents within a single parent document. This is suitable when the related data is closely associated with the parent and is not too large.

**Example:**
Suppose you have a `user` collection and each user can have multiple `posts`.

```json
// User document with embedded posts
{
  "_id": ObjectId("userId"),
  "name": "John Doe",
  "posts": [
    {
      "postId": ObjectId("postId1"),
      "title": "First Post",
      "content": "Content of the first post"
    },
    {
      "postId": ObjectId("postId2"),
      "title": "Second Post",
      "content": "Content of the second post"
    }
  ]
}
```

**Pros:**
- Simple to query when you need all data at once.
- Better performance for reads that require both parent and child data.

**Cons:**
- Limited scalability for large amounts of embedded data.
- Updates to embedded documents can be less efficient.

### 2. **Using References**

In this approach, you store the relationship between documents by referencing the `_id` of the related documents. This is suitable for larger datasets or when you need to frequently update or query the child documents independently.

**Example:**

1. **`users` Collection:**
   ```json
   {
     "_id": ObjectId("userId"),
     "name": "John Doe"
   }
   ```

2. **`posts` Collection:**
   ```json
   {
     "_id": ObjectId("postId1"),
     "userId": ObjectId("userId"),
     "title": "First Post",
     "content": "Content of the first post"
   },
   {
     "_id": ObjectId("postId2"),
     "userId": ObjectId("userId"),
     "title": "Second Post",
     "content": "Content of the second post"
   }
   ```

To query for a user and their posts, you would first find the user and then find all posts that reference that user’s `_id`.

**Pros:**
- More scalable for large datasets.
- Easier to manage and update individual documents.

**Cons:**
- Requires multiple queries or joins (using `$lookup` in aggregations) to retrieve related data.
- Slightly more complex to implement compared to embedding.

### **Query Examples**

**Embedded Documents:**
```javascript
db.users.findOne({ _id: ObjectId("userId") });
```

**References:**
1. Find user:
   ```javascript
   db.users.findOne({ _id: ObjectId("userId") });
   ```
2. Find posts for that user:
   ```javascript
   db.posts.find({ userId: ObjectId("userId") });
   ```

Alternatively, you can use the aggregation framework to perform a join-like operation:
```javascript
db.users.aggregate([
  {
    $lookup: {
      from: "posts",
      localField: "_id",
      foreignField: "userId",
      as: "posts"
    }
  }
]);
```

## Q-7. Many to Many relationship in MongoDB ?

In MongoDB, a many-to-many relationship is where multiple documents in one collection are related to multiple documents in another collection. MongoDB doesn’t enforce relationships like relational databases, so you’ll need to model these relationships using either embedded documents with arrays or references, and sometimes a combination of both.

### 1. **Using References**

For many-to-many relationships, references are often more appropriate than embedded documents due to the potential complexity and size of the data involved.

**Example:**

Consider a scenario where you have two collections: `students` and `courses`, where each student can enroll in multiple courses, and each course can have multiple students.

1. **`students` Collection:**
   ```json
   {
     "_id": ObjectId("studentId1"),
     "name": "Alice",
     "courseIds": [ObjectId("courseId1"), ObjectId("courseId2")]
   },
   {
     "_id": ObjectId("studentId2"),
     "name": "Bob",
     "courseIds": [ObjectId("courseId1")]
   }
   ```

2. **`courses` Collection:**
   ```json
   {
     "_id": ObjectId("courseId1"),
     "title": "Mathematics",
     "studentIds": [ObjectId("studentId1"), ObjectId("studentId2")]
   },
   {
     "_id": ObjectId("courseId2"),
     "title": "Science",
     "studentIds": [ObjectId("studentId1")]
   }
   ```

Here, each document in the `students` collection has an array of `courseIds`, and each document in the `courses` collection has an array of `studentIds`.

**Pros:**
- Suitable for many-to-many relationships with moderate amounts of related data.
- Avoids the complexity of embedding large arrays of documents.

**Cons:**
- Requires multiple queries to retrieve related data.
- Updates to relationships may require updates in multiple places.

### **Query Examples**

To find all courses for a particular student:

1. Find the student and retrieve their `courseIds`:
   ```javascript
   const student = db.students.findOne({ _id: ObjectId("studentId1") });
   ```

2. Find all courses that match those `courseIds`:
   ```javascript
   db.courses.find({ _id: { $in: student.courseIds } });
   ```

To find all students enrolled in a particular course:

1. Find the course and retrieve its `studentIds`:
   ```javascript
   const course = db.courses.findOne({ _id: ObjectId("courseId1") });
   ```

2. Find all students that match those `studentIds`:
   ```javascript
   db.students.find({ _id: { $in: course.studentIds } });
   ```

### 2. **Using a Junction Collection**

In cases where you need more flexibility or additional metadata about the relationship itself, you can use a junction collection (sometimes called a join table or associative array).

**Example:**

1. **`students` Collection:**
   ```json
   {
     "_id": ObjectId("studentId1"),
     "name": "Alice"
   },
   {
     "_id": ObjectId("studentId2"),
     "name": "Bob"
   }
   ```

2. **`courses` Collection:**
   ```json
   {
     "_id": ObjectId("courseId1"),
     "title": "Mathematics"
   },
   {
     "_id": ObjectId("courseId2"),
     "title": "Science"
   }
   ```

3. **`enrollments` (Junction) Collection:**
   ```json
   {
     "_id": ObjectId("enrollmentId1"),
     "studentId": ObjectId("studentId1"),
     "courseId": ObjectId("courseId1")
   },
   {
     "_id": ObjectId("enrollmentId2"),
     "studentId": ObjectId("studentId1"),
     "courseId": ObjectId("courseId2")
   },
   {
     "_id": ObjectId("enrollmentId3"),
     "studentId": ObjectId("studentId2"),
     "courseId": ObjectId("courseId1")
   }
   ```

**Pros:**
- Provides flexibility for managing many-to-many relationships.
- Allows for additional fields and metadata in the relationship itself.

**Cons:**
- Requires additional queries or aggregations to retrieve related data.

**Query Examples**

To find all courses for a particular student using the junction collection:

1. Find all enrollments for the student:
   ```javascript
   const enrollments = db.enrollments.find({ studentId: ObjectId("studentId1") });
   ```

2. Find all courses that correspond to those enrollments:
   ```javascript
   const courseIds = enrollments.map(enrollment => enrollment.courseId);
   db.courses.find({ _id: { $in: courseIds } });
   ```

To find all students enrolled in a particular course:

1. Find all enrollments for the course:
   ```javascript
   const enrollments = db.enrollments.find({ courseId: ObjectId("courseId1") });
   ```

2. Find all students that correspond to those enrollments:
   ```javascript
   const studentIds = enrollments.map(enrollment => enrollment.studentId);
   db.students.find({ _id: { $in: studentIds } });
   ```


