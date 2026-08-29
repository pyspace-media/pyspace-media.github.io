# PySpace

PySpace is a nostalgic, early-2000s-style social network built with **Python
and Django**. Think status updates, friends lists, profile comments, photo
albums, private messages... and above all, profiles you can make truly your
own with custom colors, fonts, and CSS.

This is a fresh, original project - no MySpace branding, code, or assets are
used anywhere. It just borrows the *spirit* of that era of the web.

## Features

- User accounts (signup / login / logout)
- Customizable profiles: bio, status, location, profile picture
- **Profile appearance customization**: background/panel/text/header/accent
  colors, font choice, starter theme presets, and an optional custom CSS box
  for power users - with a live preview while you edit
- Friends & friend requests (send / accept / decline / remove)
- A home feed of posts from you and your friends
- Comments and likes on posts
- Comments on user profiles ("wall" style)
- Private messages (inbox + one-on-one threads)
- Photo uploads organized into albums
- Search for other users
- Notifications for friend requests, accepted requests, likes, comments,
  profile comments, and messages

## Project layout

```
pyspace/
├── manage.py
├── requirements.txt
├── pyspace_site/      # Django project settings, root urls
├── accounts/          # Users, profiles, appearance customization, search
├── posts/             # Feed posts, comments, likes, profile comments
├── friends/           # Friend requests / friendships
├── messaging/         # Private messages
├── photos/            # Albums and photo uploads
├── notifications/     # Notification model + helper
├── templates/          # Shared/base templates
└── static/css/        # The main stylesheet
```

Each app follows standard Django structure (`models.py`, `views.py`,
`urls.py`, `forms.py`, `admin.py`, `templates/<app_name>/`), so it should feel
familiar to extend.

## Getting started

1. **Create a virtual environment** (recommended):

   ```bash
   python3 -m venv venv
   source venv/bin/activate   # on Windows: venv\Scripts\activate
   ```

2. **Install dependencies:**

   ```bash
   pip install -r requirements.txt
   ```

3. **Run migrations:**

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

4. **Create an admin account (optional, for /admin/):**

   ```bash
   python manage.py createsuperuser
   ```

5. **Run the dev server:**

   ```bash
   python manage.py runserver
   ```

6. Visit **http://127.0.0.1:8000/** and sign up!

## Notes on how customization works

Every user has a `Profile` (created automatically via a signal the moment
they sign up) with fields like `bg_color`, `panel_color`, `text_color`,
`header_color`, `accent_color`, and `font_choice`. The profile page wraps its
content in a `<div id="profile-theme">` and sets these as CSS custom
properties inline, so `static/css/style.css` can style boxes, buttons, and
text based on each user's own settings. Advanced users can also add raw CSS
in their `custom_css` field, which gets injected into a `<style>` block
scoped (by convention) to `#profile-theme`.

Starter theme presets live in `accounts/themes.py` as a plain Python
dictionary - add more presets there and they'll automatically show up in the
"Customize Appearance" dropdown.

## Roadmap ideas (not yet built)

This first version intentionally keeps things simple and working. Natural
next steps if you keep developing this on GitHub:

- Pagination/infinite scroll on the feed and photo albums
- Real-time notifications (e.g. via WebSockets/Django Channels)
- Blocking/privacy settings (private profiles, restricted messaging)
- Tests (`pytest-django` or Django's built-in `TestCase`)
- Deployment config (e.g. Docker, Postgres settings, `django-environ`)

## Deploying / developing via GitHub

This project has no external services baked in - it's a standard Django app
with SQLite by default, so it's easy to push to a new GitHub repo:

```bash
git init
git add .
git commit -m "Initial PySpace commit"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

`db.sqlite3`, `media/`, and virtual environments are already excluded via
`.gitignore`.
