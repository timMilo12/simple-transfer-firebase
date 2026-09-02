# Simple Transfer

> **Your notes, synced across all devices in real-time.**

A minimalist, real-time notes application that allows you to create, edit, and sync notes across all your devices instantly. Designed with a technical, minimal, premium, and elegant aesthetic inspired by Nothing OS, featuring a pure black interface with white text and red accents.

## Features

### Core
- **Real-Time Sync** - Changes appear instantly across all logged-in devices
- **Offline First** - Works without internet, syncs automatically when back online
- **Zero Backend Complexity** - No server to manage, no database to configure
- **Shared Accounts** - Teams can collaborate using a single username/password

### Notes Management
- Create, edit, and delete notes
- Auto-save after 500ms of inactivity
- Note pinning to keep important notes at the top
- Real-time search across titles and content
- Multiple sort modes: Recent, Created, A-Z

### Keyboard Shortcuts
| Key | Action |
|-----|--------|
| `N` | Create new note |
| `/` | Focus search input |
| `↑` | Select previous note |
| `↓` | Select next note |
| `Enter` | Open selected note |
| `Tab` | Move from title to content |
| `Esc` | Blur search input |

### Mobile Features
- Responsive design
- Collapsible sidebar
- Swipe to delete
- Touch-friendly interface

### Utility
- Copy title/content to clipboard
- Export notes as .txt files
- Character count display
- Connection status indicator

## Quick Start

### Option 1: Use as-is (for testing)
Simply open `index.html` in your browser. Note: You'll need to configure Firebase.

### Option 2: Deploy to Vercel (Recommended)

1. **Create a Firebase Project**
   - Go to [Firebase Console](https://console.firebase.google.com/)
   - Click "Add project" → Name it `Simple Transfer`
   - Enable **Authentication** and **Realtime Database**
   - In Authentication → Sign-in method:
     - Enable **Email/Password**
     - Enable **Google** (optional)
   - In Realtime Database → Create database:
     - Start in **test mode** (for development)

2. **Get Firebase Config**
   - Click ⚙️ **Project settings**
   - Scroll to "Your apps" → Click **</> (Web)**
   - Register app: Name it `Simple Transfer`
   - Copy the `firebaseConfig` object

3. **Update Code**
   In `index.html`, find and replace the Firebase config (around line 268):
   ```javascript
   const firebaseConfig = {
     apiKey: "YOUR_API_KEY",
     authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
     databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
     projectId: "YOUR_PROJECT_ID",
     storageBucket: "YOUR_PROJECT_ID.appspot.com",
     messagingSenderId: "YOUR_SENDER_ID",
     appId: "YOUR_APP_ID"
   };
   ```

4. **Deploy to Vercel**
   - Create a GitHub repository and push all files
   - Go to [Vercel Dashboard](https://vercel.com/dashboard)
   - Click "Add New" → "Project"
   - Import your GitHub repository
   - Click "Deploy"
   - Your app will be live at `https://simple-transfer.vercel.app`

5. **Secure for Production**
   In Firebase Console → Realtime Database → **Rules** tab, replace with:
   ```json
   {
     "rules": {
       "notes": {
         "$uid": {
           ".read": "auth != null && auth.uid == $uid",
           ".write": "auth != null && auth.uid == $uid"
         }
       }
     }
   }
   ```
   Click **Publish**

## Project Structure

```
simple-transfer/
├── index.html          # Complete application (single file)
├── vercel.json         # Vercel deployment configuration
├── firebase.json       # Firebase hosting configuration (optional)
└── README.md           # Documentation and setup instructions
```

## Customization

### Change the Name
Find and replace all instances of `SIMPLE TRANSFER` in `index.html`:
- Logo text
- Auth panel title
- Page title (`<title>` tag)

### Change Colors
Modify CSS variables in `index.html` (around line 25):
```css
:root {
  --bg: #000000;           /* Background */
  --text: #ffffff;         /* Primary text */
  --text-secondary: #888888; /* Secondary text */
  --text-tertiary: #555555; /* Tertiary text */
  --accent: #ff0000;       /* Accent (red) */
  --divider: #222222;     /* Divider lines */
  --hover: #111111;       /* Hover background */
  --success: #00ff00;     /* Success state (green) */
}
```

### Change Username Domain
Modify in `index.html`:
```javascript
const USERNAME_DOMAIN = 'yourdomain.com';
```

### Disable Google Sign-In
Remove or comment out the Google button section in `index.html`.

## How It Works

### Architecture
```
User Device → Firebase Auth → Firebase Realtime Database
                ↓
          index.html (Single File)
                ↓
          Vercel/Netlify/GitHub Pages
```

**No backend server required.** The entire application runs in the browser.

### Data Structure
Firebase Realtime Database structure:
```
firebase-root/
└── notes/
    └── {userId}/
        └── {noteId}/
            ├── title: string
            ├── content: string
            ├── pinned: boolean
            ├── createdAt: timestamp
            └── updatedAt: timestamp
```

### Shared Accounts
- Usernames are converted to emails using `@simpletransfer.app` domain
- Example: Username `teamname` → Email `teamname@simpletransfer.app`
- Multiple users can share one username/password for team collaboration
- All team members see and edit the same notes in real-time

## Cost

**Free Tier (sufficient for personal/team use):**
- Firebase: $0 (1GB storage, 10GB bandwidth, 50K reads/day, 20K writes/day)
- Vercel: $0 (unlimited static sites)
- **Total: $0/month**

## Security

### Production Security Rules
```json
{
  "rules": {
    "notes": {
      "$uid": {
        ".read": "auth != null && auth.uid == $uid",
        ".write": "auth != null && auth.uid == $uid"
      }
    }
  }
}
```

These rules ensure:
- Users can only read their own notes
- Users can only write to their own notes
- No unauthorized access to other users' data

## Browser Support

- Chrome (recommended)
- Firefox
- Safari
- Edge
- Mobile browsers (iOS Safari, Chrome for Android)

## License

MIT License - see [LICENSE](LICENSE) for details.
