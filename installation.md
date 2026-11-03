# Installation

LeweDesk consists of a static web frontend and a PHP API. The frontend is built with Vite and talks to the API through the `api/` folder of the same site.

## Requirements

* A web server (Apache recommended) with PHP 8.1 or newer, including the PDO and PDO\_MySQL extensions
* MySQL or MariaDB (InnoDB). Search uses FULLTEXT indexes.
* Apache `mod_rewrite` and permission to use `.htaccess` files (the API needs them for routing and to keep the `uploads/` folder private)
* Node.js and npm, to build the frontend
* A mail server (SMTP) if you want email notifications

## Get the source and build the frontend

1. Clone the repository
2. Install the dependencies:

```bash
npm install
```

3. Build the frontend:

```bash
npm run build
```

The build creates the `dist/` folder.

## Create the database

Create an empty database with the character set `utf8mb4` and a database user that has full rights on it. Write down the host, port, database name, user name and password.

Then import `db/db_init.sql` into the database. The script creates all tables and the default data: roles, permissions, issue types, status categories, status, resolutions, settings and the `admin` user. It also adds a few sample users and groups (for example _Isaac Newton_ and the group _Pilots_). Delete them in [Users](administration/system/users.md) and [Groups](administration/system/groups.md) on a live system.

{% hint style="warning" %}
The default administrator is `admin` with the password `admin`. Change the password immediately after the first login. See [Profile](user-guide/profile.md).
{% endhint %}

## Configure the environment

Create a `.env` file for the API with these entries:

| Entry                | Meaning                                                     |
| -------------------- | ----------------------------------------------------------- |
| `APP_NAME`           | Name of the application                                     |
| `APP_ENV`            | `production` for a live system                              |
| `APP_URL`            | Full URL of your installation                               |
| `DB_HOST`, `DB_PORT` | Database server and port                                    |
| `DB_NAME`            | Database name                                               |
| `DB_USER`, `DB_PASS` | Database user and password                                  |
| `JWT_SECRET`         | A long random string used to sign login tokens              |
| `SESSION_EXPIRATION` | How long a login stays valid                                |
| `MAIL_HOST`, `MAIL_PORT`, `MAIL_USER`, `MAIL_PASS` | SMTP server for notification emails |
| `MAIL_FROM`          | Sender address of notification emails                       |
| `MAX_UPLOAD_SIZE`, `ALLOWED_EXTENSIONS` | Upload settings                          |

{% hint style="danger" %}
Choose a strong, unique `JWT_SECRET` and never publish the `.env` file. Mail credentials only belong into this file, not into the system settings.
{% endhint %}

## Copy the files

Copy the contents of `dist/` and the `api/` folder to your web server. You can run LeweDesk from the root of a domain or from a subdirectory, for example `http://myserver/lewedesk`.

Make sure the `api/uploads/` folder is writable by the web server. Uploaded files are stored there under random names and are only handed out through the API after an access check.

PHP limits the size of uploads. The settings `upload_max_filesize` and `post_max_size` in your `php.ini` must be at least as large as the _Maximum Attachment Size_ you configure in the [Settings](administration/system/settings.md).

## First start

Open the URL of your installation and log in as `admin`. Then:

1. Change the password of the `admin` user
2. Open **Administration** → **System** → **Settings** and enter the _Application URL_, the _Support Email_ and your preferred defaults
3. Set the _Issue Key Prefix_ before the first issue is created. It cannot be changed afterwards.
4. Create products, tags and users, or adjust issue types, status and resolutions to your needs

Continue with the [Quick Start Guide](quick-start-guide.md).

## Sample data

For a test system, `db/db_sample_issues.sql` adds a sample product and sample issues of the sample users in all access levels and several status. Do not import it into a live system: it also locks the _Issue Key Prefix_ of the system settings.

## Housekeeping

LeweDesk cleans up after itself and needs no cron job. See [Housekeeping](administration/housekeeping.md).
