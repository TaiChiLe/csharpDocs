Let’s go over **OAuth basics**, especially for Google/Facebook login integration in Node.js apps. This is a common pattern for social logins.

---

## 1️⃣ What is OAuth?

- **OAuth 2.0** = open standard for **authorization**
    
- Allows **third-party apps** to access user data **without exposing credentials**
    
- Key concept: **token-based access**
    

**Roles in OAuth:**

1. **Resource Owner** → the user
    
2. **Client** → your app requesting access
    
3. **Authorization Server** → issues tokens (Google/Facebook)
    
4. **Resource Server** → API with user data
    

Flow:

```
User → Authorizes App → Authorization Server → Access Token → API access
```

---

## 2️⃣ Why use OAuth?

- Users don’t need to create a new account
    
- App never sees user passwords
    
- Standardized way to access APIs (Google, Facebook, GitHub, etc.)
    

---

## 3️⃣ Typical OAuth Flow (Google Example)

1. User clicks **Login with Google**
    
2. Redirect to Google’s OAuth page → user grants permission
    
3. Google redirects back to your app with **authorization code**
    
4. App exchanges code for **access token**
    
5. Use token to fetch **user profile data** from Google API
    
6. Log in or create account in your system
    

---

## 4️⃣ Node.js Implementation (with `passport.js`)

**Install dependencies:**

```bash
npm install passport passport-google-oauth20 express-session
```

### Setup Passport

```js
const passport = require("passport");
const GoogleStrategy = require("passport-google-oauth20").Strategy;

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: "/auth/google/callback"
  },
  async (accessToken, refreshToken, profile, done) => {
    // Check if user exists in DB, create if not
    let user = await User.findOne({ googleId: profile.id });
    if (!user) {
      user = await User.create({
        googleId: profile.id,
        name: profile.displayName,
        email: profile.emails[0].value
      });
    }
    return done(null, user);
  }
));

passport.serializeUser((user, done) => done(null, user.id));
passport.deserializeUser(async (id, done) => {
  const user = await User.findById(id);
  done(null, user);
});
```

---

### Express Routes

```js
const express = require("express");
const router = express.Router();
const passport = require("passport");

router.get("/auth/google",
  passport.authenticate("google", { scope: ["profile", "email"] })
);

router.get("/auth/google/callback",
  passport.authenticate("google", { failureRedirect: "/login" }),
  (req, res) => {
    // Successful login
    res.redirect("/dashboard");
  }
);

module.exports = router;
```

---

## 5️⃣ Key Concepts

|Term|Meaning|
|---|---|
|Access Token|Short-lived token to access API data|
|Refresh Token|Long-lived token to get new access tokens|
|Scopes|Define which data/app permissions are requested|
|Callback URL|URL the provider redirects to after auth|

---

## 6️⃣ Facebook OAuth

- Very similar to Google — just use `passport-facebook` strategy
    
- Replace `clientID`, `clientSecret`, and OAuth URLs with Facebook endpoints
    

---

## ✅ Summary

- OAuth allows **secure social login** without storing passwords
    
- Flow: User → Authorize → Get access token → Fetch profile → Login/Signup
    
- Use **Passport.js** or similar libraries to simplify implementation
    
- Always store **client secrets** in `.env`
    

---

