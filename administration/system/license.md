# License

Every LeweDesk installation needs a license. You manage it on the **License** tab of the [system settings](settings.md) (permission `manage_settings`). Open it from **Administration** → **System** → **Settings** → **License**.

The license is checked against the license server of the vendor (`https://lewelic.lewe.com`). LeweDesk sends the license key and the domain of your installation, nothing else.

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

| State              | Meaning                                                                                      | Application |
| ------------------ | -------------------------------------------------------------------------------------------- | ----------- |
| Active             | The license is valid for this installation.                                                  | works       |
| Pending            | The license has not been activated yet. Click **Activate**.                                  | works       |
| Not registered     | The license is valid, but this domain is not registered with it. Click **Activate**.         | works       |
| Invalid            | The license server does not know the license key. Check the key.                             | works       |
| Unavailable        | The license server could not be reached. See below.                                          | works       |
| **Expired**        | The license has expired.                                                                     | **locked**  |
| **Blocked**        | The vendor has blocked the license.                                                          | **locked**  |
| Trial              | No license key is entered; the trial period (7 days) is running.                             | works       |
| **Trial over**     | No license key is entered and the 7 days are over.                                           | **locked**  |

Administrators see a banner on every page for the states _Pending_, _Not registered_, _Invalid_, _Expired_, _Blocked_ and _Trial over_.

## Trial period

A new installation (and an existing one after the update that brought the license) runs for **7 days** without a license key. During this time every user sees the banner _This installation of LeweDesk is unlicensed. It will stop working in N days._ After the 7 days the application is locked (see below). Entering a license key ends the trial. If you remove the key later, the trial does not start again: its 7 days are counted from the first start.

## When the application is locked

If the license is _Expired_ or _Blocked_, or the trial is over:

* The login page says so. **Only administrators can log in.** Other users get the message _The license has expired. Only administrators can log in._ (This applies to the SSO login, too.)
* Visitors who are not logged in are sent to the **license expired** page. It shows the state of the license and a link to the administrator login. Only the pages _About_, _Privacy Policy_ and _Imprint_ stay open for them. The registration and the password reset are not available.
* Administrators can use the whole application, also to enter a new key or to renew the license. A red banner reminds them.
* **Sessions that are already open keep working.** Users who are logged in when the license expires are not thrown out; they only cannot log in again.
* Background work goes on: notification emails, the housekeeping and the cleanup of uploads do not depend on the license.

As soon as the license is valid again (you entered a new key, or the vendor renewed the license) the application unlocks itself. The license is checked again at the latest 30 minutes later; **Check Now** does it at once.

## If the license server cannot be reached

LeweDesk keeps the last known state of the license. A license that was _Active_ stays usable when the license server or your internet connection is down. The panel then shows _Last contact with the license server failed_ with the reason, and LeweDesk tries again after 5 minutes. The license is checked every 30 minutes otherwise.

The connection needs outbound HTTPS from the web server to the license server. PHP uses the `curl` extension; if it is missing or fails (for example because the server has no CA certificates configured), PHP streams (`allow_url_fopen`) are used instead.

## Server configuration

Three values in the `.env` file of the server set up the connection to the license server. They are part of the delivered configuration; change them only if the vendor tells you to.

| Key              | Description                                                                 | Default                        |
| ---------------- | --------------------------------------------------------------------------- | ------------------------------ |
| `LICENSE_SERVER` | URL of the license server                                                   | `https://lewelic.lewe.com`     |
| `LICENSE_ITEM`   | The product name                                                            | `LeweDesk`                     |
| `LICENSE_SECRET` | The product secret that identifies LeweDesk to the license server           | set in the delivered `.env`    |

The panel says so when `LICENSE_SERVER` or `LICENSE_SECRET` is missing.

## Audit log

Changes of the license are written to the [Audit Log](audit-log.md) (category _Configuration_): `license.key_saved`, `license.activated`, `license.deregistered`, `license.status_changed` (with the old and new state) and `license.unavailable` (once for every outage of the license server). The license key is masked there (`ABCD...1234`). A login that was refused because of the license is a failed `auth.login_failed` with the reason `license_expired`.

## Troubleshooting

| Message                                                   | What to do                                                                                              |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| _The license key is unknown._                             | Check the key for typing errors and save it again.                                                      |
| _The license is not activated for this domain._           | Click **Activate**.                                                                                      |
| _The license server could not be reached._                | Check the internet access of the web server (firewall, proxy). The last known state stays in force.    |
| _The license server refused the product secret._          | `LICENSE_SECRET` in the `.env` file is wrong or missing.                                                 |
| _The product is switched off on the license server._      | Contact the vendor.                                                                                      |
| _Too many requests to the license server._               | Wait a moment and try again.                                                                             |
