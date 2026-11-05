# Profile

<figure><img src="../.gitbook/assets/profile.png" alt=""><figcaption></figcaption></figure>

Open your profile from the user menu in the top bar (click your avatar, then **Profile**).

<figure><img src="../.gitbook/assets/menu-user.png" alt=""><figcaption></figcaption></figure>

## Profile tab

| Field                | Description                                                          |
| -------------------- | -------------------------------------------------------------------- |
| Username             | Your login name (cannot be changed)                                  |
| Email                | Your email address. It must be unique. Notifications are sent here.  |
| First name, Last name | Your name                                                           |
| Current password     | Required to change your password                                     |
| Password             | Enter a new password to change it (at least 4 characters). Leave empty to keep your current password. |
| Confirm new password | Repeat the new password                                              |

Click **Save**.

If your administrator uses [LDAP authentication](../administration/system/ldap-authentication.md), your password is managed outside of LeweDesk. The password fields are then replaced with a note and cannot be used. Change your password where you changed it before.

The same applies if you sign in with SSO ([OIDC authentication](../administration/system/oidc-authentication.md)). The 2FA tab is not shown then, because two factor authentication is handled by your identity provider.

## Avatar tab

<figure><img src="../.gitbook/assets/profile-avatar.png" alt=""><figcaption></figcaption></figure>

Your avatar is shown in the top bar and next to your name on issues.

* **Upload your own**: PNG, JPG, GIF or WEBP, up to 1 MB. The image is shown as a circle, so a square image works best.
* **Sample avatars**: click an image to use it.

## Notifications tab

Choose per event whether you get an email and/or an in-app notification. See [Notifications](notifications.md).

## 2FA tab

Set up two factor authentication with an authenticator app, or disable it again (this needs your password). See [Two Factor Authentication](two-factor-authentication.md).

## Options tab

<figure><img src="../.gitbook/assets/profile-options.png" alt=""><figcaption></figcaption></figure>

| Option                         | Description                                                                   |
| ------------------------------ | ----------------------------------------------------------------------------- |
| Show status background color   | Colors the row background of issue tables according to the issue's status     |
