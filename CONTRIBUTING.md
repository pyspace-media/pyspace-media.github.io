# Contributing to PySpace

Thanks for your interest in contributing! PySpace is a community, open-source
project and welcomes issues, bug reports, and pull requests.

## Ground rules

- Be respectful. See our expectations for behavior in all project spaces
  (issues, PRs, discussions) — harassment, hate speech, and personal attacks
  are not tolerated.
- PySpace is an **original** project. Do not submit code, assets, logos, or
  design elements copied from MySpace or any other proprietary platform.
- Keep the nostalgic, customizable spirit of the project in mind when
  proposing UI changes — dense, boxy, colorful, personality-driven layouts
  are a feature, not a bug, but everything should still be usable on modern
  devices.

## Getting set up

1. Fork the repository and clone your fork.
2. Follow the **Local development** steps in `README.md` to get the project
   running.
3. Create a branch for your change: `git checkout -b fix/short-description`.

## Project structure

Each major feature lives in its own Django app under `apps/`:

| App             | Responsibility                                   |
|------------------|--------------------------------------------------|
| `accounts`       | Registration, auth, account settings             |
| `profiles`       | Profile data + customization (themes, CSS, etc.) |
| `friends`        | Friend requests and friendships                  |
| `posts`          | Feed posts, likes                                |
| `comments`       | Post comments and profile "wall" comments        |
| `messaging`      | Private conversations                            |
| `media`          | Photo albums and uploads                         |
| `notifications`  | Cross-app notification feed                      |
| `moderation`     | Reporting, blocking, rate limiting, mod queue     |

Try to keep changes scoped to the relevant app(s), and avoid adding
cross-app imports beyond what's already established (e.g. `posts` calling
`apps.notifications.services.notify`).

## Making changes

- Write clear, readable code with comments where the "why" isn't obvious.
- Add or update tests under `apps/<app>/tests.py` for any behavior change.
- Run the test suite before opening a PR:

  ```bash
  python manage.py test
  ```

- Run Django's system checks:

  ```bash
  python manage.py check
  ```

- If you change models, generate migrations and commit them:

  ```bash
  python manage.py makemigrations
  ```

- Keep commits focused; a PR that does one thing is much easier to review
  than one that mixes a feature with unrelated refactors.

## Submitting a pull request

1. Push your branch and open a PR against `main`.
2. Describe what the change does and why, and link any related issue.
3. Include screenshots for UI changes when practical — this is a very
   visual project.
4. Be responsive to review feedback; small back-and-forth is normal.

## Reporting bugs / requesting features

Open a GitHub issue with:

- What you expected to happen vs. what actually happened
- Steps to reproduce (for bugs)
- Your environment (OS, Python version, browser) if relevant

## Security issues

Please do not open a public issue for security vulnerabilities. Instead,
contact the maintainers privately (see the repository's security policy,
if one is configured) so the issue can be addressed before public
disclosure.
