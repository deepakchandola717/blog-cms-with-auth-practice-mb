### ** Blog CMS with Authentication**

This project involves building a blog CMS (Content Management System) where users can read, write, and manage blogs. It will have authentication to restrict certain features like writing blogs to logged-in users. The backend will use MongoDB for data storage.

---

### **Frontend Requirements**

#### **1. Home Page**
- The homepage should display a list of blogs.
  - Each blog should show its **title**, **author name**, and a short **excerpt** of the content.
  - A **"Read More"** button should be present for each blog, redirecting to a detailed view of the selected blog.
- The homepage should fetch blog data from the backend API.
- Include navigation links in header for:
  - Login/Signup
  - Write Blog (only visible if the user is logged in)

---

#### **2. Blog Details Page**
- Create a dynamic route (e.g., `/blogs/[id]`) to display the full content of a selected blog.
- The page should include:
  - Blog title
  - Full content of the blog
  - Author name
  - Published date
- The blog data should be fetched from the backend API using the blog’s unique ID.

---

#### **3. Login and Signup Pages**
- **Login Page**:
  - A form with fields for **username** and **password**.
  - On successful login, the user should be redirected to the homepage.
  - Display an error message for invalid credentials.
- **Signup Page**:
  - A form with fields for **username** and **password**.
  - On successful signup, the user should be redirected to the login page.

---

#### **4. Write Blog Page**
- This page should allow logged-in users to create a new blog.
  - The page should have a form with fields for:
    - Blog title
    - Blog content
  - The form should submit the data to the backend API.
- Include a message or redirect on successful blog creation.
- If an unauthenticated user tries to access this page, redirect them to the login page.

---

#### **5. User Authentication Handling**
- Store user authentication tokens securely:
  - Use **HttpOnly cookies** to store the token (preferred for security).
- Ensure users cannot access the "Write Blog" page unless they are authenticated.
- Show a "Logout" button in the navigation for authenticated users:
  - Clicking "Logout" should clear the authentication token and redirect to the homepage.

---

### **Backend Requirements**

#### **1. Database Setup (MongoDB)**
- **Collections**:
  - **Users Collection**:
    - Fields:
      - `username`: A unique string to identify the user.
      - `password`: A hashed string (to securely store user passwords).
  - **Blogs Collection**:
    - Fields:
      - `title`: The blog’s title.
      - `content`: The full content of the blog.
      - `author`: The username of the user who created the blog.
      - `createdAt`: A timestamp of when the blog was created.

---

#### **2. API Routes**
- **Authentication Routes**:
  - `/api/register`: 
    - Accepts `username` and `password`.
    - Hash the password before storing it in the database.
    - Returns success or error based on the result.
  - `/api/login`:
    - Accepts `username` and `password`.
    - Validates the user’s credentials.
    - If valid, returns a JWT token and sets it as a secure HttpOnly cookie.
    - Returns an error message for invalid credentials.

- **Blog Routes**:
  - `/api/blogs` (GET):
    - Fetch and return a list of all blogs.
    - Each blog should include its title, author, and an excerpt of the content.
  - `/api/blogs` (POST):
    - Accepts blog data (`title`, `content`) from authenticated users.
    - Requires the user’s authentication token to associate the blog with the author.
    - Returns success or error based on the result.
  - `/api/blogs/[id]` (GET):
    - Fetch and return a specific blog by its unique ID.
    - Includes full content, title, author name, and creation timestamp.

---

#### **3. Middleware**
- **Authentication Middleware**:
  - Validate the user’s authentication token (JWT) in API requests that require authentication (e.g., creating a blog).
  - Return an error response for requests with invalid or missing tokens.
- **Error Handling**:
  - Provide meaningful error responses for failed authentication, missing required fields, or invalid API requests.

---

### **Security Requirements**
1. **Password Security**:
   - Hash passwords before storing them in the database (e.g., using `bcrypt`).
2. **Token Security**:
   - Use JWT for user authentication.
   - Store tokens securely in **HttpOnly cookies** to prevent cross-site scripting (XSS) attacks.
3. **Input Validation**:
   - Validate all user inputs (e.g., during registration, login, and blog creation) to prevent injection attacks.

---

### **Deployment Requirements**
1. **Frontend**:
   - Deploy the Next.js application on a platform like **Vercel**.
2. **Backend**:
   - Host the MongoDB database on **MongoDB Atlas**.
   - Ensure the backend API routes are secured and hosted alongside the frontend (Next.js API routes handle this natively).
3. **Environment Variables**:
   - Use `.env` to manage sensitive data such as the MongoDB connection string and JWT secret.

---

### **Additional Features (Optional)**
- **Pagination**:
  - Implement pagination for the list of blogs on the homepage.
- **Blog Editing/Deleting**:
  - Allow authenticated users to edit or delete their own blogs.
- **Search**:
  - Add a search bar on the homepage to filter blogs by title or content.
- **User Profiles**:
  - Create a profile page for each user showing their created blogs.
