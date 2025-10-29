# CourseFlow API Documentation

**Version:** 1.0  
**Date:** October 29, 2025  
**Base URL:** `/api` (e.g., `http://localhost:3000/api`)  
**Authentication:** JWT-based (Bearer Token in Authorization header for protected endpoints).  
**Content Type:** JSON (unless specified otherwise, e.g., multipart for uploads).  
**Error Format:** `{ "error": "Message", "details": optional }` with appropriate HTTP status codes.

This document outlines the RESTful API endpoints for the CourseFlow backend. The API handles course uploads, student interaction tracking, AI analysis, and recommendations. All endpoints are prefixed with `/api`.

For setup and usage, refer to [README.md](../README.md). For project overview, see [PRD.md](../PRD.md).

## Table of Contents

- [Authentication](#authentication)
- [Endpoints](#endpoints)
  - [User Authentication](#user-authentication)
  - [Course Management](#course-management)
  - [Content Upload and Analysis](#content-upload-and-analysis)
  - [Student Interactions](#student-interactions)
  - [Recommendations](#recommendations)
  - [Analytics](#analytics)
- [Error Handling](#error-handling)
- [Rate Limiting](#rate-limiting)
- [Examples](#examples)

## Authentication

Most endpoints require authentication. Use JWT tokens obtained from login/register.

- **Token Usage:** Include in headers: `Authorization: Bearer <token>`
- **Expiration:** Tokens expire after 1 hour (refresh via /refresh endpoint if implemented).
- **Public Endpoints:** Only /auth/register and /auth/login are public.

## Endpoints

### User Authentication

#### POST /auth/register
**Description:** Register a new user (course creator).  
**Request Body:**
```json
{
  "email": "string",
  "password": "string",
  "name": "string" (optional)
}
```
**Response:** 201 Created
```json
{
  "user": { "id": "string", "email": "string" },
  "token": "string"
}
```

#### POST /auth/login
**Description:** Login and get JWT token.  
**Request Body:**
```json
{
  "email": "string",
  "password": "string"
}
```
**Response:** 200 OK
```json
{
  "user": { "id": "string", "email": "string" },
  "token": "string"
}
```

### Course Management

#### POST /courses
**Description:** Create a new course entry.  
**Request Body:**
```json
{
  "title": "string",
  "description": "string" (optional)
}
```
**Response:** 201 Created
```json
{
  "course": { "id": "string", "title": "string", "createdAt": "date" }
}
```

#### GET /courses
**Description:** List user's courses.  
**Query Params:** `limit` (int, default 10), `page` (int, default 1)  
**Response:** 200 OK
```json
{
  "courses": [
    { "id": "string", "title": "string", "status": "string" }
  ],
  "total": "int"
}
```

#### GET /courses/:id
**Description:** Get course details.  
**Response:** 200 OK
```json
{
  "id": "string",
  "title": "string",
  "contentAnalysis": "object" (optional),
  "recommendations": "array" (optional)
}
```

#### DELETE /courses/:id
**Description:** Delete a course.  
**Response:** 204 No Content

### Content Upload and Analysis

#### POST /upload
**Description:** Upload course content files for analysis. Supports multipart/form-data.  
**Request:**
- Form fields: `courseId` (string, required), `files` (array of files: PDF, DOCX, PPTX, videos, etc.)
**Response:** 200 OK
```json
{
  "files": [
    { "id": "string", "filename": "string", "analysis": "object" (AI results) }
  ],
  "message": "Upload and analysis complete"
}
```
**Notes:** Triggers AI analysis automatically. File size limit: 100MB per file.

#### GET /analysis/:fileId
**Description:** Retrieve AI analysis for a specific file.  
**Response:** 200 OK
```json
{
  "analysis": {
    "structure": "object",
    "readability": "number",
    "topics": "array",
    "difficultyCurve": "array"
  }
}
```

### Student Interactions

#### POST /track
**Description:** Track student interactions (e.g., page views, clicks).  
**Request Body:**
```json
{
  "courseId": "string",
  "userId": "string" (student ID),
  "interactionType": "string" (e.g., "page_view", "click", "skip"),
  "element": "string" (e.g., "video-3"),
  "duration": "number" (optional, in seconds),
  "timestamp": "date"
}
```
**Response:** 201 Created
```json
{ "message": "Interaction tracked" }
```
**Notes:** Used by frontend tracking SDK. Batched requests supported via array.

#### GET /interactions/:courseId
**Description:** Get aggregated interactions for a course.  
**Query Params:** `from` (date), `to` (date), `type` (string)  
**Response:** 200 OK
```json
{
  "interactions": "array",
  "summary": {
    "totalViews": "int",
    "avgDuration": "number",
    "dropOffPoints": "array"
  }
}
```

### Recommendations

#### GET /recommendations/:courseId
**Description:** Fetch AI-powered recommendations for a course.  
**Query Params:** `filter` (string, e.g., "high-impact"), `sort` (string, e.g., "effort-asc")  
**Response:** 200 OK
```json
{
  "recommendations": [
    {
      "id": "string",
      "type": "string" (e.g., "structural"),
      "description": "string",
      "effort": "string" (low/medium/high),
      "impact": "number" (1-10)
    }
  ]
}
```

#### POST /recommendations/:id/apply
**Description:** Mark a recommendation as applied (for tracking).  
**Request Body:**
```json
{ "notes": "string" (optional) }
```
**Response:** 200 OK
```json
{ "message": "Recommendation applied" }
```

### Analytics

#### GET /analytics/:courseId
**Description:** Get course analytics (engagement, outcomes).  
**Response:** 200 OK
```json
{
  "engagement": "number" (percentage),
  "completionRate": "number",
  "heatmap": "object" (drop-offs, hot spots),
  "trends": "array" (over time)
}
```

## Error Handling

- **400 Bad Request:** Invalid input (e.g., missing fields).
- **401 Unauthorized:** Missing or invalid token.
- **403 Forbidden:** Insufficient permissions.
- **404 Not Found:** Resource not found.
- **500 Internal Server Error:** Unexpected errors (logged server-side).

Example Error Response:
```json
{
  "error": "Invalid course ID",
  "details": "Course not found in database"
}
```

## Rate Limiting

- 100 requests per minute per IP/user.
- Headers: `X-Rate-Limit-Remaining`, `X-Rate-Limit-Reset`.

## Examples

### cURL Upload Example
```bash
curl -X POST http://localhost:3000/api/upload \
  -H "Authorization: Bearer <token>" \
  -F "courseId=abc123" \
  -F "files=@/path/to/course.pdf"
```

### JavaScript Fetch Track Example
```javascript
fetch('/api/track', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer <token>'
  },
  body: JSON.stringify({
    courseId: 'abc123',
    interactionType: 'page_view',
    duration: 120
  })
});
```

For more code snippets, see [CODE_SNIPPETS.md](../CODE_SNIPPETS.md).

---

This API is designed to be extensible. Contributions to improve or add endpoints are welcome—see [CONTRIBUTING.md](../CONTRIBUTING.md). If you encounter issues, open a ticket on [GitHub](https://github.com/decagondev/course-flow/issues).
