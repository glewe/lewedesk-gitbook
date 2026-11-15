# Issues

An issue is a report, a question or a request. LeweDesk calls what other systems call tickets "issues".

## Issue key

Every issue has a key made of the key of its product and a number, for example **LD-123**. Every product has its own key and counts its own numbers: the first issue of a product with the key _WEB_ is **WEB-1**. The administrator sets the keys (see [Products](../../administration/issues/products.md)).

The key changes when an issue is [moved](issue-actions.md#move) to another product, but the old key keeps working: links, bookmarks and mails with the old key still lead to the issue. Old links of the form `issue.html?id=12` are read as _LD-12_.

When you write an issue key in a description or a comment, it is linked automatically (if the reader may see that issue).

## Properties

| Property    | Description                                                                                                 |
| ----------- | ----------------------------------------------------------------------------------------------------------- |
| Type        | Kind of issue, e.g. Bug or Question                                                                         |
| Priority    | How urgent the issue is: Critical, High, Normal (default) or Low                                            |
| Status      | Where the issue is in its life, e.g. Open or Resolved                                                       |
| Resolution  | Why the issue is done. Empty until the issue is resolved.                                                   |
| Summary     | One-line title                                                                                              |
| Description | Full description in [Markdown](../markdown.md)                                                              |
| Product | The product the issue is about (required). Its key is the prefix of the issue key. |
| Affected version | The version of the product the issue occurs in (optional) |
| Fixed version | The version of the product that fixes the issue (optional). The issue is listed in the release notes of that version. |
| Access      | Who may see the issue: Public, Registered or Reporter only                                                  |
| Reporter    | The user who created the issue                                                                              |
| Assignee    | The Agent who takes care of the issue                                                                       |
| Watchers    | Users who follow the issue. The reporter, the assignee and everybody who comments are added automatically. |
| Tags        | Free labels                                                                                                 |
| Created / Updated | Date and time of creation and of the last change                                                      |

## Access

| Access        | Who can see the issue                                          |
| ------------- | -------------------------------------------------------------- |
| Public        | Everybody, visitors who are not logged in included             |
| Registered    | All logged in users (default)                                  |
| Reporter only | Only the reporter and the support team                         |

The support team (Agents and Administrators) can always see all issues.

## In this chapter

* [List Issues](list-issues.md): quick views, filters and search
* [Create Issue](create-issue.md)
* [Issue Detail](issue-detail.md): reading an issue
* [Issue Actions](issue-actions.md): Respond, Resolve, Close, Reopen, Assign, Watch and more
* [Edit Issue](edit-issue.md): for the support team
* [Comments](comments.md)
* [Attachments](attachments.md)
* [History](history.md)
* [SLA](sla.md): response and resolution targets, due times and breaches
