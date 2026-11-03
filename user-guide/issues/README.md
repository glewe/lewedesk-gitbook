# Issues

An issue is a report, a question or a request. LeweDesk calls what other systems call tickets "issues".

## Issue key

Every issue has a key made of a prefix and a number, for example **LETI-123**. The numbering is global. The administrator sets the prefix (see [Settings](../../administration/system/settings.md)). When you write an issue key in a description or a comment, it is linked automatically (if the reader may see that issue).

## Properties

| Property    | Description                                                                                                 |
| ----------- | ----------------------------------------------------------------------------------------------------------- |
| Type        | Kind of issue, e.g. Bug or Question                                                                         |
| Status      | Where the issue is in its life, e.g. Open or Resolved                                                       |
| Resolution  | Why the issue is done. Empty until the issue is resolved.                                                   |
| Summary     | One-line title                                                                                              |
| Description | Full description in [Markdown](../markdown.md)                                                              |
| Product     | The product the issue is about (optional)                                                                   |
| Version     | The product version (optional)                                                                              |
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
