# Housekeeping

LeweDesk cleans up after itself. The following jobs run automatically inside the application, at most every five minutes and after a response was sent, so they do not slow down your users:

| Job                    | What it does                                                                                                     |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Mail queue             | Notification emails are queued and sent right after the response. A failed mail is retried once; a second failure is written to the log. |
| Old notifications      | In-app notifications are deleted after the number of days set in _Notification Retention (Days)_                 |
| Temporary uploads      | Images that were uploaded while writing a new issue but never saved are deleted after _Temporary Upload Cleanup (Hours)_ |
| Old mails              | Processed entries of the mail queue are deleted                                                                   |

The limits are set in the [Settings](system/settings.md) (tab _Issue Config_).

## Running it from cron (optional)

If you prefer a fixed schedule, run the housekeeping from the command line of the server, for example from cron:

```bash
php api/housekeeping.php
```

To clean temporary uploads only:

```bash
php api/cleanup_uploads.php
```

Both scripts run on the command line only. Calling them through the web server answers with an error.

## Maintenance mode

To work on the system without users interfering, switch on _Under Maintenance_ in the [Settings](system/settings.md) (tab _System_). Only administrators can use the application then. Everybody else sees the maintenance page.
