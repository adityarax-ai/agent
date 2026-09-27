# GSuiteEventAttachment

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Calendar.Models.GSuiteEventAttachment`

Represents a Google Calendar event attachment — a reference to a Google Drive file (Google Calendar only supports Drive file references as attachments, not uploaded content).

**Assembly:** `UiPath.GSuite`

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `Title` | `string` | Attachment title (usually the file name). |
| `FileUrl` | `string` | URL link to the attachment (the Drive view link). |
| `MimeType` | `string` | Internet media type (MIME type) of the attachment. |
| `IconLink` | `string` | URL link to the attachment's icon (populated by Google; read-only at the API). |
| `FileId` | `string` | ID of the attached Google Drive file (populated by Google; read-only at the API). |

## Notes

- This type appears as elements in the `Attachments` collection of [GSuiteEventItem](GSuiteEventItem.md). It is an **output** model — to attach files to an event, pass [`GDriveRemoteItem`](GDriveRemoteItem.md) values to the attachment inputs of Create Event / Modify Event instead.
- Only Drive **files** can be attached to events; folders are not supported by the Google Calendar API.

## Used By

Activities that return or accept this type -- see activity docs for details.
