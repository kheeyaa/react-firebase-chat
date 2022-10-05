# React Firebase Super Chat

A simple fullstack chat demo with React and Firebase.

Watch on full [React Firebase Chat Tutorial](https://youtu.be/zQyrwxMPm88) on YouTube.

[Live demo](https://fireship-demos.web.app/)

## firestore/rules

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if false;
    }
    match /messages/{docId} {
      allow read: if request.auth.uid != null;
      allow create: if canCreateMessage();
    }
    function canCreateMessage() {
      let isSignedIn = request.auth.uid != null;
      let isOwner = request.auth.uid == request.resource.data.uid;
      let isNotBanned = exists(
        /databases/$(database)/documents/banned/$(request.auth.uid)
      ) == false;
      return isSignedIn && isOwner && isNotBanned;
    }
  }
}
```
