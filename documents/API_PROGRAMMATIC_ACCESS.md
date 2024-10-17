
# Supercharge.info API - Programmatic Access

## Metadata

- **Document Version**: 1.0
- **Last Updated**: October 2024
- **Associated Software Release**: Supercharge.info API v2.0
- **Document Author**: The Supercharge.info Team
- **Contact Information**: support@supercharge.info

## Changelog

- **v1.0** (October 2024): Initial version, including programmatic endpoints for `/changes/delete` and `/changes/restoreAdded`, authentication requirements, and example requests.

## Introduction
This document provides a guide to interacting with the *supercharge.info* API for programmatic purposes. These endpoints allow for modifying data and interacting with the supercharging station logs.

## Table of Contents
1. [Authentication & Permissions](#authentication-permissions)
2. [Endpoints Overview](#endpoints-overview)
3. [Endpoints](#endpoints)
4. [Example Requests](#example-requests)
5. [Error Handling](#error-handling)

## Authentication & Permissions
Some of the programmatic endpoints require authentication and specific user roles. If you attempt to interact with these endpoints without proper authentication or permissions, you will receive a `401 Unauthorized` or `403 Forbidden` error.

- **Login Required**: Some endpoints require the user to be logged in to perform actions.
- **Role Requirements**: For certain actions, like deleting or restoring logs, the user must have the `editor` role.

## Endpoints Overview
The following endpoints allow for creating, modifying, or deleting data:

- `/changes/delete`: Deletes a specific change log entry (requires `POST` and user authentication).
- `/changes/restoreAdded`: Restores a previously deleted change log entry.

## Endpoints

### Endpoint: `/changes/delete`

**Description**: Deletes a change log entry.

**Method**: `POST`

**Authentication Required**: `YES`

**Required Role**: `editor`

**Parameters**:
| Parameter   | Type   | Required | Description           |
|-------------|--------|----------|-----------------------|
| `changeId`  | Integer| YES      | The unique ID of the change log to delete. |

**Response**:
```json
{
  "message": "Change log entry deleted successfully.",
  "changeId": 12345
}
```

### Endpoint: `/changes/restoreAdded`

**Description**: Restores a previously deleted change log entry.

**Method**: `POST`

**Authentication Required**: `YES`

**Required Role**: `editor`

**Parameters**:
| Parameter   | Type   | Required | Description           |
|-------------|--------|----------|-----------------------|
| `siteId`    | Integer| YES      | The unique ID of the site to restore. |

**Response**:
```json
{
  "message": "Change log entry restored successfully.",
  "siteId": 54321
}
```

## Example Requests

### Example: Delete a Change Log Entry

**Request**:
```
POST /changes/delete
{
  "changeId": 12345
}
```

**Response**:
```json
{
  "message": "Change log entry deleted successfully.",
  "changeId": 12345
}
```

### Example: Restore a Deleted Change Log Entry

**Request**:
```
POST /changes/restoreAdded
{
  "siteId": 54321
}
```

**Response**:
```json
{
  "message": "Change log entry restored successfully.",
  "siteId": 54321
}
```

## Error Handling
The API will return standard HTTP error codes. Some of the common ones include:

- `400 Bad Request`: Invalid parameters were provided.
- `401 Unauthorized`: Authentication is required or has failed.
- `403 Forbidden`: You do not have permission to perform this action.
- `404 Not Found`: The specified resource could not be found.
- `500 Internal Server Error`: The server encountered an error while processing the request.
