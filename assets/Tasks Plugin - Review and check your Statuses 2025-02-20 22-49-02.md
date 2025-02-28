# Review and check your Statuses

## About this file

This file was created by the Obsidian Tasks plugin (version 7.16.0) to help visualise the task statuses in this vault.

If you change the Tasks status settings, you can get an updated report by:

- Going to `Settings` -> `Tasks`.
- Clicking on `Review and check your Statuses`.

You can delete this file any time.

## Status Settings

<!--
Switch to Live Preview or Reading Mode to see the table.
If there are any Markdown formatting characters in status names, such as '*' or '_',
Obsidian may only render the table correctly in Reading Mode.
-->

These are the status values in the Core and Custom statuses sections.

| Status Symbol | Next Status Symbol | Status Name | Status Type   | Problems (if any)                                                                                              |
| ------------- | ------------------ | ----------- | ------------- | -------------------------------------------------------------------------------------------------------------- |
| `space`       | `B`                | Action      | `TODO`        |                                                                                                                |
| `x`           | `space`            | Done        | `DONE`        |                                                                                                                |
| `d`           | `?`                | In Progress | `IN_PROGRESS` |                                                                                                                |
| `C`           | `space`            | Cancelled   | `CANCELLED`   |                                                                                                                |
| `?`           | `-`                | Testes      | `IN_PROGRESS` |                                                                                                                |
| `B`           | `d`                | Planeamento | `IN_PROGRESS` |                                                                                                                |
| `-`           | `space`            | Terminado   | `DONE`        | For information, the conventional type for status symbol `-` is `CANCELLED`: you may wish to review this type. |

## Loaded Settings

<!-- Switch to Live Preview or Reading Mode to see the diagram. -->

These are the settings actually used by Tasks.

```mermaid
flowchart LR

classDef TODO        stroke:#f33,stroke-width:3px;
classDef DONE        stroke:#0c0,stroke-width:3px;
classDef IN_PROGRESS stroke:#fa0,stroke-width:3px;
classDef CANCELLED   stroke:#ddd,stroke-width:3px;
classDef NON_TASK    stroke:#99e,stroke-width:3px;

1["'Action'<br>[ ] -> [B]<br>(TODO)"]:::TODO
2["'Done'<br>[x] -> [ ]<br>(DONE)"]:::DONE
3["'In Progress'<br>[d] -> [?]<br>(IN_PROGRESS)"]:::IN_PROGRESS
4["'Cancelled'<br>[C] -> [ ]<br>(CANCELLED)"]:::CANCELLED
5["'Testes'<br>[?] -> [-]<br>(IN_PROGRESS)"]:::IN_PROGRESS
6["'Planeamento'<br>[B] -> [d]<br>(IN_PROGRESS)"]:::IN_PROGRESS
7["'Terminado'<br>[-] -> [ ]<br>(DONE)"]:::DONE
1 --> 6
2 --> 1
3 --> 5
4 --> 1
5 --> 7
6 --> 3
7 --> 1

linkStyle default stroke:gray
```
