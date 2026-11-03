# Upgrading

## Before you start

* Make a backup of the database and of the `api/uploads/` folder
* Read the release information on the **About** page (_Help_ → _About_ → _Release Information_) of the new version

## Upgrade steps

1. Build the new version (`npm install` and `npm run build`)
2. Copy the new contents of `dist/` and the `api/` folder to your web server. Keep your existing `.env` file and the `api/uploads/` folder.
3. Update the database. Later versions can add tables. Run this on the command line of the server:

```bash
php api/setup_db.php
```

The script only adds what is missing and keeps your data. It runs on the command line only. Calling it through the web server answers with an error.

4. Clear your browser cache or reload the page with Ctrl+F5, so that the new frontend files are used

{% hint style="info" %}
If you cannot run PHP on the command line, import the new tables from the current `db/db_init.sql`. The script only creates missing tables and adds missing default records, so running it again is safe.
{% endhint %}
