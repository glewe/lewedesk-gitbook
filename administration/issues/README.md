# Issues

These pages maintain the lists that structure your issues.

| Page                                    | Used for                                                                    |
| --------------------------------------- | --------------------------------------------------------------------------- |
| [Custom Views](custom-views.md)         | Saved filter combinations of the issue list of all users                    |
| [Issue Types](issue-types.md)           | What kind of issue it is (Bug, Feature, Question, ...)                      |
| [Priorities](priorities.md)             | How urgent an issue is (Critical, High, Normal, Low)                        |
| [Products](products.md)                 | The product an issue is about                                               |
| [Status](status.md)                     | Where an issue is in its life                                               |
| [Status Categories](status-categories.md) | Groups of status that drive the quick views                               |
| [Resolutions](resolutions.md)           | Why an issue is done                                                        |
| [Tags](tags.md)                         | Free labels to find and group issues                                        |

## System records

Issue types, status, status categories, resolutions and priorities come with **system records** that the issue workflow depends on. A system record

* cannot be deleted or deactivated
* has a locked name (the name is translated into the user's language)
* can still get its own description, icon and colors

You can add your own records next to them.

## Common fields

Most of these forms share the following fields:

| Field            | Description                                                                                                |
| ---------------- | ---------------------------------------------------------------------------------------------------------- |
| Preview          | Shows how the record looks in lists and on issues while you edit it                                        |
| Name             | Unique name (max. 50 characters)                                                                           |
| Description      | Short description of what the record is for                                                                |
| Icon             | Bootstrap Icons class, e.g. `bi-bug`. Leave empty for no icon.                                             |
| Color            | Color of the icon and the text                                                                             |
| Background color | Background color. It is displayed with transparency so that it works in light and dark mode.               |
| Sort order       | Position in lists and drop-down menus (lowest first). Leave empty to put a new record at the end.          |
| Active           | Inactive records cannot be selected for new issues but stay on existing issues                             |

## Deleting records in use

When you delete an issue type, status, resolution or priority that issues still use, LeweDesk asks you to select a **replacement**. All affected issues get the replacement. When you delete a product or a tag, it is simply removed from the issues.
