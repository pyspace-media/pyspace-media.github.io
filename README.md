# PySpace

PySpace is an original, open-source social networking site inspired by the
customizable, chaotic, personal feel of social networks from the
2005–2010 era. It is **not** affiliated with, and does not reuse any
branding, logos, or copyrighted design elements from, any real platform —
it's its own thing, built from scratch with Python and Django.

Highly personalized profiles. Friends. A feed. Comments. Private messages.
Photo albums. Glitter optional.

## Features

- **Accounts** — registration, login/logout, password reset, privacy settings
- **Profiles** — bio, interests, favorite music/movies/books, status message,
  personal links, and deep customization (themes, fonts, colors, background
  images, sanitized custom CSS, allow-listed media embeds)
- **Friends** — requests, accept/decline, friends lists, mutual friends
- **Posts** — text + image posts, likes, comments, a friends-only feed
- **Profile comments** — leave a comment directly on someone's page
- **Messaging** — private inbox, conversations, read/unread state
- **Media** — photo albums with captions and upload validation
- **Notifications** — friend requests, accepts, comments, likes, messages
- **Moderation & safety** — reporting, blocking, staff moderation queue,
  basic rate limiting on posts/comments/likes

## Tech stack

- **Backend:** Python 3.12+, Django 5.0 (LTS)
- **Database:** PostgreSQL in production, SQLite for local development
- **Frontend:** Django templates, vanilla CSS/JS (no build step required)
- **Auth:** Django's built-in authentication system, with a custom `User` model
- **Media:** Pillow for image handling

## Project layout

```
pyspace/
├── apps/
│   ├── accounts/       # registration, login, settings
│   ├── profiles/       # profile data + customization
│   ├── friends/        # friend requests & friendships
│   ├── posts/          # feed posts & likes
│   ├── comments/       # post comments & profile comments
│   ├── messaging/       # private messages
│   ├── media/          # photo albums (Django app label: media_app)
│   ├── notifications/  # cross-app notifications
│   └── moderation/     # reports, blocks, rate limiting
├── pyspace_project/    # Django project settings, root urls, wsgi/asgi
├── static/             # CSS/JS/images
├── templates/           # base.html (app templates live inside each app)
├── manage.py
├── requirements.txt
├── .env.example
└── README.md            (you are here)
```

## Local development

### Prerequisites

- Python 3.12 or newer
- pip

### Setup

```bash
# 1. Clone the repo
git clone https://github.com/<your-org>/pyspace.git
cd pyspace

# 2. Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure environment variables
cp .env.example .env
# Edit .env if you want to change anything - the defaults work out of
# the box for local development (SQLite, DEBUG=True).

# 5. Run migrations
python manage.py migrate

# 6. Create an admin account
python manage.py createsuperuser

# 7. Run the dev server
python manage.py runserver
```

Visit `http://127.0.0.1:8000/` to see the feed (you'll be redirected to log
in first), or `http://127.0.0.1:8000/admin/` for the Django admin.

### Running tests

```bash
python manage.py test
```

### Database setup

By default PySpace uses SQLite (`db.sqlite3`), which needs no setup and is
fine for local development. To use PostgreSQL instead (recommended for
anything beyond local dev), set `DATABASE_URL` in your `.env`:

```
DATABASE_URL=postgres://username:password@localhost:5432/pyspace
```

Then create the database (e.g. `createdb pyspace`) and run
`python manage.py migrate` as usual.

## Deployment

PySpace ships with sane production defaults gated behind `DEBUG=False`
(HTTPS redirect, secure cookies, HSTS, etc. — see `pyspace_project/settings.py`).

General steps for deploying to any standard host (Railway, Render, Fly.io,
a VPS, etc.):

1. Provision a PostgreSQL database and set `DATABASE_URL`.
2. Set `SECRET_KEY` to a strong, unique value (never reuse the dev key).
3. Set `DEBUG=False` and `ALLOWED_HOSTS` to your real domain(s).
4. Set `CSRF_TRUSTED_ORIGINS` to your HTTPS origin(s) if you're behind a
   reverse proxy.
5. Install dependencies: `pip install -r requirements.txt`.
6. Collect static files: `python manage.py collectstatic --noinput`.
7. Run migrations: `python manage.py migrate`.
8. Serve with a production WSGI server, e.g.:

   ```bash
   pip install gunicorn
   gunicorn pyspace_project.wsgi:application --bind 0.0.0.0:8000
   ```

9. Put a real web server or CDN in front for static/media files in
   high-traffic deployments (WhiteNoise is a lightweight option if you'd
   rather not stand up a separate static file server — add it to
   `requirements.txt` and `MIDDLEWARE` if you go this route).

Connecting the repository to a hosting platform generally means: point the
platform at this GitHub repo, set the environment variables above in its
dashboard, and let it run `pip install -r requirements.txt` followed by the
migrate/collectstatic/gunicorn commands as its build & start steps. Exact
steps vary by platform — consult your host's Python/Django deployment guide.

## Security notes

- Never commit a real `.env` file, API keys, or credentials. `.gitignore`
  already excludes `.env`.
- Custom profile CSS is server-side sanitized (see
  `apps/profiles/forms.py::sanitize_custom_css`) to block scripts and other
  dangerous constructs, and is scoped to the profile's own container.
- Media embeds are restricted to an allow-list of domains
  (YouTube, SoundCloud, Bandcamp, Spotify).
- Uploaded images are validated for file type and size
  (`apps/media/models.py::validate_image_file`).
- A lightweight cache-based rate limiter guards posting, commenting, and
  liking against basic spam/abuse (`apps/moderation/middleware.py`).

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md)
before opening a pull request.

## License

PySpace is released under the [MIT License](LICENSE).
