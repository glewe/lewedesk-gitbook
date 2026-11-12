# License

Every LeweDesk installation needs a license. You manage it on the **License** tab of the [system settings](settings.md) (permission `manage_settings`). Open it from **Administration** → **System** → **Settings** → **License**.

The license is checked against the license server of the vendor (`https://lewelic.lewe.com`). LeweDesk sends the license key, the domain of your installation and the product name, nothing else; no data of your users or issues. The Privacy Policy page of your installation (Help menu) describes this in section 3.9. The terms of use are in the `LICENSE` file delivered with LeweDesk.

## Enter and activate the license key

1. Open the **License** tab.
2. Enter the license key you received in the field **License Key** and click **Save Key**. LeweDesk checks the key with the license server at once.
3. Click **Activate**. This registers the domain of your installation with the license. A license that is still _Pending_ becomes _Active_.

The panel at the top shows the state of the license and its details: product, license key, customer, company, expiry date (with the days left), the number of registered installations ("1 of 3") and the domain of your installation.

| Button             | What it does                                                                                                                                                                                  |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Save Key**       | Saves the license key and checks it. An empty key removes the license key.                                                                                                                      |
| **Activate**       | Activates the license for this installation and registers its domain with the license server. If the maximum number of installations of the license is reached, the oldest registration is replaced. |
| **Deregister**     | Removes the domain of this installation from the license, for example before you move LeweDesk to another server. The place is free for another installation. The license is then no longer valid for this installation until you register it again. |
| **Check Now**      | Asks the license server for the current state of the license at once.                                                                                                                            |

{% hint style="info" %}
The domain that is registered is the server name of the web server (`SERVER_NAME`), for example `desk.example.com`. If you move LeweDesk to a new domain, deregister the old domain first (or let the oldest registration be replaced) and click **Activate** on the new one.
{% endhint %}

## The expiry warning

The setting **Expiry Warning (Days)** (default **30**) tells how many days before the license expires administrators are warned. They then see a yellow banner on every page: _The license expires in N days._ Enter `0` for no warning. This setting is saved with **Save Settings** like all others.

## States of the license

| State              | Meaning                                                                                      | Application                  |
| ------------------ | -------------------------------------------------------------------------------------------- | ---------------------------- |
| Active             | The license is valid for this installation.                                                  | works                        |
| Unavailable        | The license server could not be reached. See below.                                          | works                        |
| Trial              | No license key is entered; the trial period (14 days) is running.                            | works, with a banner         |
| Pending            | The license has not been activated yet. Click **Activate**.                                  | grace period (14 days)       |
| Not registered     | The license is valid, but this domain is not registered with it. Click **Activate**.         | grace period (14 days)       |
| Invalid            | The license server does not know the license key. Check the key.                             | grace period (14 days)       |
| Expired            | The license has expired.                                                                     | grace period (14 days)       |
| **Trial over**     | No license key is entered and the 14 days are over.                                          | **limited mode**             |
| **Blocked**        | The vendor has blocked the license.                                                          | **limited mode** at once     |

## Trial and grace period

**Trial.** A new installation (and an existing one after the update that brought the license) runs for **14 days** without a license key. If you remove the key later, the trial does not start again: its 14 days are counted from the first start. Entering a valid license key ends the trial.

**Grace period.** If the license is _Invalid_, _Not registered_, _Pending_ or _Expired_, everything keeps working for **14 days**:

* _Expired_: the 14 days start the day after the expiry date.
* _Invalid_, _Not registered_ and _Pending_: the 14 days start with the first check that returned this state. They start again only after the license has been _Active_ in between.
* _Blocked_ has no grace period: the limited mode starts at once.

During the trial and the grace period **administrators** (only they) see a yellow banner on every page, for example _Your LeweDesk license is missing. Your grace period ends in 12 days._ The banner names the reason (missing, invalid, not registered for this domain, not activated yet, expired) and links to the License tab. The other users see nothing and nothing is limited.

## Limited mode

When the trial or the grace period is over, or the license is blocked, LeweDesk runs in the **limited mode**. Nothing is deleted or changed, and nobody is locked out of their data. Administrators see a red banner on every page that names the reason.

| Feature                  | In the limited mode                                                                                                                                                       |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Issues                   | With **50 or more** issues (deleted ones do not count) no new issue can be created. **Create issue** is disabled. Existing issues can still be edited, commented and deleted. |
| Users                    | With **20 or more** users (all users except the built-in `admin` count, inactive ones too) no new user can be created. **Add** is disabled. Existing users can still be edited, deactivated and deleted. |
| Self-registration        | Clicking **Sign up** on the login page shows the message _This feature is currently disabled due to an invalid license._ The setting _Allow User Self-Registration_ keeps its value. |
| Two factor authentication | Nobody can set up 2FA, and _Force Two Factor Authentication_ is disabled and has no effect (its value is kept). Users who already have 2FA keep it and can switch it off. |
| LDAP and SSO             | Existing logins keep working. LeweDesk does not create users from LDAP or SSO anyway, so nothing else changes.                                                              |
| Audit Log                | The Audit Log cannot be opened (page, menu entry and API). Entries are still written and the retention cleanup goes on.                                                      |
| Custom Views             | Custom views cannot be used (list bar, profile tab, administration page). The saved views are kept.                                                                          |

Everything else works as usual, including comments, login, password reset, the data export and the notification emails.

As soon as a valid license is activated, **all limits are lifted at once**. Nothing was deleted or changed in the limited mode, so everything is back as it was. The license is checked again at the latest 30 minutes later; **Check Now** does it at once.

## Locking the application (optional)

By default the application is never locked. If you want the old, strict behavior, set `LICENSE_LOCK=1` in the `.env` file of the server. Then, after the grace period (and for a blocked license at once), **only administrators can log in**, visitors who are not logged in are sent to the **license expired** page (only _About_, _Privacy Policy_ and _Imprint_ stay open), and registration and password reset are not available. Sessions that are already open keep working. This lock replaces the limited mode.

## If the license server cannot be reached

LeweDesk keeps the last known state of the license. A license that was _Active_ stays usable when the license server or your internet connection is down. The panel then shows _Last contact with the license server failed_ with the reason, and LeweDesk tries again after 5 minutes. The license is checked every 30 minutes otherwise.

The connection needs outbound HTTPS from the web server to the license server. PHP uses the `curl` extension; if it is missing or fails (for example because the server has no CA certificates configured), PHP streams (`allow_url_fopen`) are used instead.

## Server configuration

Three values in the `.env` file of the server set up the connection to the license server. They are part of the delivered configuration; change them only if the vendor tells you to. A fourth, optional value, `LICENSE_LOCK`, is described above.

| Key              | Description                                                                 | Default                        |
| ---------------- | --------------------------------------------------------------------------- | ------------------------------ |
| `LICENSE_SERVER` | URL of the license server                                                   | `https://lewelic.lewe.com`     |
| `LICENSE_ITEM`   | The product name                                                            | `LeweDesk`                     |
| `LICENSE_SECRET` | The product secret that identifies LeweDesk to the license server           | set in the delivered `.env`    |

The panel says so when `LICENSE_SERVER` or `LICENSE_SECRET` is missing.

## Audit log

Changes of the license are written to the [Audit Log](audit-log.md) (category _Configuration_): `license.key_saved`, `license.activated`, `license.deregistered`, `license.status_changed` (with the old and new state) and `license.unavailable` (once for every outage of the license server). The license key is masked there (`ABCD...1234`). With `LICENSE_LOCK=1`, a login that was refused because of the license is a failed `auth.login_failed` with the reason `license_expired`.

## Troubleshooting

| Message                                                   | What to do                                                                                              |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| _The license key is unknown._                             | Check the key for typing errors and save it again.                                                      |
| _The license is not activated for this domain._           | Click **Activate**.                                                                                      |
| _The license server could not be reached._                | Check the internet access of the web server (firewall, proxy). The last known state stays in force.    |
| _The license server refused the product secret._          | `LICENSE_SECRET` in the `.env` file is wrong or missing.                                                 |
| _The product is switched off on the license server._      | Contact the vendor.                                                                                      |
| _Too many requests to the license server._               | Wait a moment and try again.                                                                             |
