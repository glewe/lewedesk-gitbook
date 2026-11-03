# System

The system pages manage who may use LeweDesk and how it behaves.

| Page                          | Purpose                                                          |
| ----------------------------- | ---------------------------------------------------------------- |
| [Users](users.md)             | User accounts, their roles and groups                            |
| [Roles](roles.md)             | Sets of permissions: Administrator, Agent, User, Public and your own |
| [Groups](groups.md)           | Collections of users that can be granted permissions together    |
| [Permissions](permissions.md) | What a role or group may do                                      |
| [Settings](settings.md)       | Application name, email, theme, issue limits and more            |

## How access works

1. A **permission** allows one thing, for example `create_issue`.
2. Permissions are granted to **roles** and to **groups**.
3. A **user** has one or more roles and may belong to groups. The user holds every permission of their roles and groups.
4. Visitors who are not logged in hold the role _Public_.

Administrators bypass all permission checks.
