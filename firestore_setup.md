# 🔥 Firestore Setup Guide: sudokas.live

To ensure your database works correctly and securely with the new code, please follow these steps in your Firebase Console.

## 1. Security Rules
Go to **Firestore Database > Rules**. 

> [!IMPORTANT]
> **NÃO copies** os acentos graves (\`\`\`) nem a palavra "javascript". Copia apenas o código abaixo (desde `rules_version` até ao último `}`):

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Profiles: Users can read all (for leaderboard) but only write their own
    match /profiles/{userId} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Purchases: Users can only see and create their own records
    match /purchases/{purchaseId} {
      allow create: if request.auth != null && request.resource.data.user_id == request.auth.uid;
      allow read: if request.auth != null && resource.data.user_id == request.auth.uid;
    }
    
    // Chat: Anyone can read messages, authenticated users can send
    match /chat_messages/{messageId} {
      allow read: if true;
      allow create: if request.auth != null;
    }

    // Config: Publicly readable, but only admins can write
    // To enable Admin: You must manually add a field { "is_admin": true } 
    // to the user's profile document in the console.
    match /config/{document} {
      allow read: if true;
      allow write: if request.auth != null && get(/databases/$(database)/documents/profiles/$(request.auth.uid)).data.is_admin == true;
    }
  }
}
```

## 2. Collection Structure (Reference)
As coleções serão criadas automaticamente pelo App.

### `profiles`
- **Doc ID**: UID do Utilizador.
- **Campos**: `name`, `credits`, `total_score`, etc.

### `chat_messages`
- **Campos**: `text`, `user_id`, `user_name`, `timestamp`.

### `purchases`
- **Campos**: `user_id`, `credits`, `amount`, `currency`, `created_at`.
