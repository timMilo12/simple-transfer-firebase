# Simple Transfer

> **Your notes, synced across all devices in real-time.**

A minimalist, real-time notes application that allows you to create, edit, and sync notes across all your devices instantly. Designed with a technical, minimal, premium, and elegant aesthetic inspired by Nothing OS, featuring a pure black interface with white text and red accents.

**Now powered by Supabase** - A free, open-source Firebase alternative with real-time sync, authentication, and zero backend code.

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

### Option 1: Deploy to Vercel (Recommended - 5 minutes)

#### Step 1: Create Supabase Project
1. Go to **[supabase.com](https://supabase.com/)** and sign in with GitHub
2. Click **"New Project"** → **"Create new organization"** (if prompted)
3. **Project name:** `simple-transfer`
4. **Database password:** `YourStrongPassword123` (remember this!)
5. **Region:** Choose closest to you
6. Click **"Create project"** → Wait 2-3 minutes

#### Step 2: Set Up Database Table
1. In your Supabase dashboard, click **"Table Editor"** (left sidebar)
2. Click **"Create a new table"**
3. **Table name:** `notes`
4. Add these columns:

| Column Name | Type | Default | Notes |
|-------------|------|---------|-------|
| `id` | UUID | `gen_random_uuid()` | Primary key |
| `user_id` | UUID | | References `auth.users.id` |
| `title` | TEXT | `'Untitled'` | |
| `content` | TEXT | `''` | |
| `pinned` | BOOLEAN | `false` | |
| `created_at` | TIMESTAMPTZ | `now()` | |
| `updated_at` | TIMESTAMPTZ | `now()` | |

5. Click **"Save"**

6. **Enable Row Level Security (RLS):**
   - Click **"Authentication"** → **"Policies"** (left sidebar)
   - Click **"Create Policy"**
   - **Policy name:** `Allow users to access their own notes`
   - **Table:** `notes`
   - **Using SQL expression:**
     ```sql
     auth.uid() = user_id
     ```
   - **Enable for:** SELECT, INSERT, UPDATE, DELETE
   - Click **"Save"**

#### Step 3: Get Your Supabase Credentials
1. In your project dashboard, click **⚙️ Project Settings** (bottom left)
2. Click **"API"** tab
3. **Copy these exactly:**
   - **Project URL:** `https://your-project-ref.supabase.co`
   - **anon key:** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`

#### Step 4: Update the Code
1. In your GitHub repository, edit `index.html`
2. **Find lines 257-258** (search for `SUPABASE_URL`)
3. **Replace with your credentials:**
   ```javascript
   const SUPABASE_URL = 'https://your-project-ref.supabase.co';
   const SUPABASE_ANON_KEY = 'your-supabase-anon-key';
   ```
4. **Commit the change**

#### Step 5: Deploy to Vercel
1. Go to **[Vercel Dashboard](https://vercel.com/dashboard)**
2. Click **"Add New"** → **"Project"**
3. Click **"Import"** next to your GitHub repository (`timMilo12/simple-transfer-firebase`)
4. Click **"Deploy"**
5. Wait ~30 seconds...

**🎉 Your app will be live at:** `https://simple-transfer-firebase.vercel.app`

---

### Option 2: Test Locally
1. Clone this repository
2. Create a Supabase project (Steps 1-3 above)
3. Update `index.html` with your credentials
4. Open `index.html` in your browser

> Note: Realtime sync requires the app to be served (not just opened as a file). Use `python -m http.server 8000` or similar.

---

## Project Structure

```
simple-transfer/
├── index.html          # Complete application (single file)
├── vercel.json         # Vercel deployment configuration
├── firebase.json       # (Legacy, can be removed)
└── README.md           # Documentation and setup instructions
```

## How It Works

### Architecture
```
User Device → Supabase Auth → Supabase Database (PostgreSQL)
                ↓
          index.html (Single File)
                ↓
          Vercel/Netlify/GitHub Pages
```

**No backend server required.** The entire application runs in the browser, communicating directly with Supabase services.

### Data Structure
Supabase PostgreSQL table structure:
```sql
CREATE TABLE notes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES auth.users(id),
    title TEXT DEFAULT 'Untitled',
    content TEXT DEFAULT '',
    pinned BOOLEAN DEFAULT false,
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ DEFAULT now()
);
```

### Security
- **Row Level Security (RLS)** ensures users can only access their own notes
- **Anon key** is public and safe to use in frontend code
- **Authentication** via Supabase Auth (email/password, Google, etc.)

### Shared Accounts
- Usernames are converted to emails using `@simpletransfer.app` domain
- Example: Username `teamname` → Email `teamname@simpletransfer.app`
- Multiple users can share one username/password for team collaboration
- All team members see and edit the same notes in real-time

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
Remove or comment out the Google button section in `index.html`:
```html
<!-- <div class="social-auth">
  <button type="button" class="social-btn google-btn" id="googleSignIn">
    <svg class="social-icon" viewBox="0 0 24 24">...</svg>
    SIGN IN WITH GOOGLE
  </button>
</div> -->
```

## Browser Support

- Chrome (recommended)
- Firefox
- Safari
- Edge
- Mobile browsers (iOS Safari, Chrome for Android)

## Cost

**Free Tier (sufficient for personal/team use):**
- Supabase: $0 (500MB database, 2GB bandwidth, 50K rows)
- Vercel: $0 (unlimited static sites)
- **Total: $0/month**

## Troubleshooting

### "Realtime subscription status: 403"
- Make sure RLS is enabled on the `notes` table
- Verify your anon key is correct

### Notes not appearing
- Check the browser console for errors
- Verify the `user_id` column is populated correctly
- Ensure RLS policy allows SELECT on `notes`

### Google sign-in not working
- In Supabase dashboard, go to **Authentication** → **Providers**
- Enable Google provider
- Add your Vercel URL to **Site URL** in Authentication settings

### Offline mode not working
- Supabase realtime requires an internet connection
- For true offline support, consider using localStorage as a cache (not implemented in this version)

## Migration from Firebase

If you were using the Firebase version:
1. Export your Firebase data
2. Create a Supabase project
3. Import your data into the `notes` table
4. Update the credentials in `index.html`
5. Deploy

## License

MIT License - see [LICENSE](LICENSE) for details.
