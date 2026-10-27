# My Tasks: hosting guide

This folder has everything you need to put the task app on Netlify or GitHub Pages:

- `index.html` is the whole app.
- `README.md` is this guide.

The app already contains all 37 tasks, with their order, levels, deadlines and Sinhala/Tamil translations, as they were when this copy was made.

There are two ways to run it:

| Mode | What it does | Setup |
|---|---|---|
| **This device only** (default) | Works right away. Changes are saved only in the browser that made them, so your team and your sir won't see each other's changes. | None |
| **Shared live list** (Firebase) | Everyone sees the same list, and changes appear for everyone instantly, just like the Claude version. | About 10 minutes, free |

---

## Step 1: Put it online

### Option A: Netlify (easiest, no account needed to start)

1. Go to **https://app.netlify.com/drop**
2. Drag this whole folder (the one containing `index.html`) onto the page.
3. Netlify gives you a link like `https://something-random.netlify.app`. That's your site.
4. To keep the site and change its name, create a free Netlify account and claim the site. Then go to **Site configuration → Change site name**.

To update the site later, open the site in Netlify, go to **Deploys**, and drag the folder again.

### Option B: GitHub Pages

1. Sign in at **https://github.com** and click **New repository**. Give it a name such as `tasks`, make it **Public**, and click **Create repository**.
2. Click **uploading an existing file**, drag in `index.html` and `README.md`, then click **Commit changes**.
3. Go to **Settings → Pages**. Under **Branch**, choose `main` and `/ (root)`, then click **Save**.
4. After a minute or two, your site will be at `https://YOUR-USERNAME.github.io/tasks/`.

To update the site later, upload the new `index.html` again in the repository.

---

## Step 2 (recommended): Make it one shared live list with Firebase

Without this step, each person's changes stay on their own device.

1. Go to **https://console.firebase.google.com** and sign in with a Google account.
2. Click **Create a project**, give it a name such as `my-tasks`, and click through the steps. You can turn off Google Analytics.
3. In the left menu, open **Build → Firestore Database** and click **Create database**.
   - **Location:** choose `asia-south1 (Mumbai)`, the closest to Sri Lanka.
   - Choose **Start in production mode**, then click **Create**.
4. Open the **Rules** tab, replace everything with the rules below, and click **Publish**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /tasks/{taskId} {
         allow read, write: if true;
       }
     }
   }
   ```

5. Click the **gear icon → Project settings**. Under **Your apps**, click the **web icon `</>`**, give the app a nickname, and click **Register app**.
6. Firebase shows a block of code containing `const firebaseConfig = { ... }`. Copy only the part from `{` to `}`.
7. Open `index.html` in a text editor such as Notepad or VS Code. Near the top, find this line:

   ```
   window.FIREBASE_CONFIG = null;
   ```

   Replace `null` with what you copied, so it looks like this:

   ```
   window.FIREBASE_CONFIG = {
     apiKey: "AIza...",
     authDomain: "my-tasks.firebaseapp.com",
     projectId: "my-tasks",
     storageBucket: "my-tasks.appspot.com",
     messagingSenderId: "1234567890",
     appId: "1:1234567890:web:abc123"
   };
   ```

8. Save the file and upload it again to Netlify or GitHub, following Step 1.
9. Open your site once. The first time, it automatically fills the shared list with all your tasks. From then on, everyone sees the same live list.

---

## Sharing with your sir

- **Normal link** (can edit): `https://your-site.netlify.app/`
- **View-only link** (can see tasks, progress and dashboard, but can't change anything): add `?view` to the end of the address, like `https://your-site.netlify.app/?view`

Send your sir the view-only link if he only needs to follow progress.

## Good to know

- **Anyone who has the normal link can edit the list.** Only share it with your team. The `?view` link hides the editing buttons, but it doesn't add a password. For real logins, Firebase Authentication can be added later.
- **The Claude version and this hosted version are separate lists.** Once you move to the hosted version, have everyone use only that link, so changes don't end up in two places.
- **Translation:** all current tasks are already translated into Sinhala and Tamil. Automatic translation of *new* tasks only works inside Claude, so on Netlify or GitHub, new tasks show in the language they were typed in. All buttons and labels are still fully translated.
- **Firebase cost:** the free "Spark" plan is more than enough for a team task list.
