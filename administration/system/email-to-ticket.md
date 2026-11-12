# Email to Ticket

LeweDesk can read a support mailbox and turn the mails into issues and comments:

* A mail from a user to the support address becomes an **issue**. The subject is the summary, the text is the description, the attachments are attached.
* An **answer to a notification mail** (or any mail with the issue key in the subject, e.g. _LD-123_) becomes a **comment** on that issue. The quoted history and the signature are cut off.
* The sender gets a **confirmation** with the issue key. Mails about an issue carry the line _Please type your reply above this line_, so that answers can be told from the history that mail programs quote.

A mail is handled **as the user it comes from**, with exactly that user's rights: a user who may not create issues, or may not see an issue, cannot do it by mail either. Nothing is ever deleted from the mailbox: handled mails are moved to the folder `Processed`, all others to `Rejected`.

{% hint style="info" %}
Email to Ticket is part of the license. After the trial and grace period without a valid license it stops reading the mailbox (the mails stay where they are). See [License](license.md).
{% endhint %}

## Setting it up

The mailbox and its login are configured in the `.env` file in the folder of the application, like the mail sending and LDAP. This way the settings survive an upgrade and the password is never stored in the database.

| Key | Description | Default |
| --- | ----------- | ------- |
| `MAILIN_YES` | `1` switches Email to Ticket on | `0` |
| `MAILIN_HOST` | The IMAP server, e.g. `imap.example.com` (not the SMTP server) | none |
| `MAILIN_PORT` | `993` for implicit TLS, `143` for STARTTLS | `993` |
| `MAILIN_ENCRYPTION` | `ssl` (implicit TLS), `tls` (STARTTLS) or `none` | follows the port |
| `MAILIN_USER`, `MAILIN_PASS` | The login of the mailbox. Only needed if they differ from `MAIL_USER` and `MAIL_PASS` of the mail sending: normally the same mailbox sends and receives. | `MAIL_USER`, `MAIL_PASS` |
| `MAILIN_FOLDER` | The folder that is read | `INBOX` |
| `MAILIN_PROCESSED_FOLDER` | Mails that became an issue or a comment are moved here. It is created if it does not exist. | `Processed` |
| `MAILIN_REJECTED_FOLDER` | Mails that were refused are moved here. It is created if it does not exist. | `Rejected` |
| `MAILIN_VALIDATE_CERT` | `0` skips the check of the certificate (self-signed test servers only) | `1` |
| `MAILIN_AUTH_SERVER` | The name of **your** mail server in the `Authentication-Results` header (see below) | none |

Example:

```
MAILIN_YES=1
MAILIN_HOST=imap.ionos.de
MAILIN_AUTH_SERVER=kundenserver.de
```

Write each setting on its own line and do not put a comment behind a value. The `.env` file is read as it is: in `MAILIN_PROCESSED_FOLDER=Processed # moved mails` the folder would be called `Processed # moved mails`. Put comments on a line of their own, starting with `#`.

Then open **Settings > Email to Ticket**. The tab shows whether the feature is on, the mailbox, the last check and what happened in the last 24 hours. **Test Connection** logs in to the mailbox and looks at it without changing anything. **Pick Up Now** reads the mailbox at once, without waiting for the next check, and shows what it did (issues, comments, rejected, failed). The check interval starts again from that moment. If another check is just working on the mailbox, the button tells you so and you can press it again a moment later.

{% hint style="warning" %}
Use a mailbox of its own for the support address. The folders `Processed` and `Rejected` are made in it, and the mails in the folder that is read are moved out of it.
{% endhint %}

### When the mailbox is read

There is no cron job. After a request to the application, LeweDesk reads the mailbox if the last check is longer ago than the **Check Interval** (at least 60 seconds, 5 minutes by default). Open browser pages ask for notifications all the time, so there is always some traffic while somebody works with the application. If you prefer a fixed schedule, run `php api/housekeeping.php` from a cron job: it reads the mailbox, too. After you sent a test mail, use **Pick Up Now** on the settings tab instead of waiting.

## Who may write

A mail is accepted when its **sender address belongs to a user** and the address is believed.

Anybody can write any address into the From line of a mail. So LeweDesk wants the proof of your own mail server that the mail really comes from that address: the header `Authentication-Results`, which a mail server adds after it checked SPF, DKIM and DMARC. But a sender can add such a header himself, so only the header written by **your** server counts, and it is recognized by the first word of the header, the name of the server. Set this name in `MAILIN_AUTH_SERVER`:

```
php scripts/mailin-auth-sample.php .env
```

reads the names from the newest mails of the mailbox (only these header lines, nothing else) and tells which one to use. For IONOS it is `kundenserver.de`. A mail is verified when the server wrote `dmarc=pass`, or `spf=pass` together with `dkim=pass`.

Without `MAILIN_AUTH_SERVER`, no mail is verified. The setting **Trust the Sender Address** then lets users in without proof, but never support staff and administrators, whose mails could do the most harm.

## Settings

| Setting | Description | Default |
| ------- | ----------- | ------- |
| Check Interval | Seconds between two checks | 300 |
| Issue Type, Product | Type and product of the issues made from mails (the first active type if none is chosen) | first active type, no product |
| Access | Who sees the issues made from mails. _Reporter only_ is the safe choice. | Reporter only |
| Unknown Senders | What happens with mails from addresses that no user has (see below) | Reject |
| Confirmation Mail | Send the sender the key of the issue | on |
| Trust the Sender Address | See above | off |
| Mails per Check | The most mails handled in one check; the others wait | 20 |
| Largest Text (KB) | The text of a mail is cut off after this size | 256 |
| Mails per Hour | The most mails that may become issues or comments within an hour, in total. The others wait in the mailbox. 0 means no limit. | 100 |
| Allowed / Blocked Sender Domains | Only these domains are handled / these are always rejected. Subdomains are included. | empty |

The hourly limits per user (_Issue Config_) apply to mails as they do to the browser.

## Unknown senders

| Setting | What happens |
| ------- | ------------ |
| Reject | The mail is moved to `Rejected`. Nobody is told. |
| Make an account | An account with the role _User_ is made from the address (the username from the part before the @, the name from the From line). The person gets a mail with a link to set a password, valid for 7 days. At most 10 accounts per hour. Not while LDAP or SSO is active: then administrators make the accounts. |
| Anonymous | The issue belongs to the account _Email requester_, and the address of the sender is kept with the issue (support staff see it in the issue details). Only the confirmation goes to the sender: the notifications of LeweDesk do not reach somebody who has no account. |

Both ways other than _Reject_ need the proof of your mail server, whatever _Trust the Sender Address_ says. A sender who is not a user can start an issue but never answer an issue of somebody else.

## What is checked

A mail is moved to `Rejected` (and the reason is in the log) when

* it is an automatic mail: auto-reply, delivery report, list or bulk mail, or from the address of the application itself (so that two systems cannot answer each other forever),
* the sender domain is blocked or not on the list of allowed domains,
* the sender is not a user (see above), or the address could not be verified,
* the sender may not do what the mail asks (create an issue, comment, see that issue),
* it answers a mail about an **internal comment**: an answer could become a public comment by mistake,
* the hourly limit of the sender is reached (the mail is rejected) or the limit for the whole mailbox (the mail waits).

Details:

* A reply to a **closed** issue adds its comment, but no attachments; a **deleted** issue takes nothing.
* A reply is only a comment: it does not change the status of the issue.
* **Attachments** are checked like uploads in the browser: allowed types (txt, md, pdf, zip, gif, jpg, png), size, content, and the limits per issue. A file that is not accepted never stops the mail: the issue or comment is made and a note says which files were left out.
* Mails above 10 MB are not read.
* A mail that fails because of a problem on the server (for example the database) stays in the mailbox and is tried again, three times in all, then it is rejected.
* A mail is never handled twice: its Message-ID is logged. If LeweDesk is stopped in the middle of a mail, that mail is not tried again (it may have created something), and the log says so.

## The log

The tab **Email to Ticket** lists the latest 30 mails with the result (issue, comment, rejected, failed) and the reason. The log is kept as long as the [Audit Log](audit-log.md). Rejected mails and problems with the mailbox also appear in the audit log in the category _Email to ticket_ (events `mail.rejected`, `mail.fetch_failed`, `mail.limit_hit`, `mail.user_created`).

In the issue list, issues made from mails show an envelope next to their key, and a filter **Source** shows only those or only the others. The issue details name the source.

## Problems

| Symptom | Check |
| ------- | ----- |
| The tab says _Off_ | `MAILIN_YES=1` in the `.env` file, and a host and a login. The host is the IMAP server (`imap.…`), not the SMTP server. |
| _Test Connection_ fails | Host, port and encryption; the password; whether the host allows connections to the IMAP port (shared hosts sometimes block it). `php scripts/mailin-spike.php .env` shows what the host offers. |
| Nothing happens | The feature needs traffic or a cron job (see above), the license, and unseen mails in the folder `MAILIN_FOLDER`. Use **Pick Up Now** to read at once. |
| A mail is in the inbox but is not picked up | Only unseen mails are read. A mail that was opened in webmail or in another mail program is marked as read and skipped: mark it as unread again. |
| The tab still shows an old error after you fixed the `.env` | The error is the result of the last check. It is replaced by the next one: use **Pick Up Now**, or wait for the interval. **Test Connection** does not replace it. |
| Folders with odd names like _Processed # …_ appeared | A comment was written behind the folder name in the `.env` file. Remove it, move the mails into the right folders and delete the wrong ones. |
| Email to Ticket was working and is off after an upgrade or a deployment | The deployment replaced the `.env` file. Keep the `MAILIN_*` lines in the file that is deployed, and check `MAILIN_YES=1` and `MAILIN_AUTH_SERVER` again. |
| Mails of users are rejected as _could not be verified_ | `MAILIN_AUTH_SERVER` is missing or wrong, or the mail server did not pass SPF/DKIM/DMARC for that mail. Look at the header `Authentication-Results` of such a mail. |
| Answers become new issues | The answer carries neither the Message-ID of a notification (`In-Reply-To`, `References`) nor the key of the issue in the subject. Mail programs normally keep both. |
| The mailbox fills up with mails from the application | The notification _issue created_ goes to the support address of the Issue Config. If that is the mailbox that is read, those mails are rejected as _from the application itself_. Use another support address. |
