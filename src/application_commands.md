# Application Commands

- [Overview](#overview)
- [Definitions](#definitions)
  - [Application Command Types](#application-command-types)

---

## Overview

Application commands are one the many new ways that you can use to interact with
your application. Application commands focus on removing the complexities of
traditional message parsing, thus making the behavior of bots more consistent
and streamlined. There are currently _3_ types of application commands that you
can expect to see:

- **Slash commands** can be invoked by typing `/` into the chat field. A menu
  will appear showing a list of commands for each application.
- **Message commands** and **user commands** can be invoked by right-clicking on
  their respective target and then navigating to the `Apps` option on the
  context menu.

## Definitions

### Application Command Types

| Name      | Value | Description                                                                          |
| --------- | ----- | ------------------------------------------------------------------------------------ |
| `SLASH`   | `1`   | An application command invoked by typing `/` into the chat field.                    |
| `USER`    | `2`   | An application command invoked from the context menu by right-clicking on a user.    |
| `MESSAGE` | `3`   | An application command invoked from the context menu by right-clicking on a message. |
