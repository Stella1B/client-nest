# Social Media APIs Documentation

## Overview

This document provides comprehensive documentation for the Social Media APIs in the Client-Nest platform. The APIs handle post creation, comment management, engagement tracking, hashtag management, and media file uploads.

## Base URL

```
http://localhost:8000/api/social/
```

## Authentication

All endpoints require JWT authentication. Include the token in the Authorization header:

```
Authorization: Bearer <your_jwt_token>
```

## Endpoints

### 1. Post Management

#### 1.1 List Posts
**GET** `/posts/`

Get all posts with filtering, search, and pagination.

**Query Parameters:**
- `post_type`: Filter by post type (text, image, video, link, poll)
- `visibility`: Filter by visibility (public, private, followers)
- `author`: Filter by author ID
- `is_pinned`: Filter by pinned status
- `is_featured`: Filter by featured status
- `search`: Search in content, hashtags, mentions
- `ordering`: Sort by created_at, updated_at, like_count, comment_count, view_count
- `page`: Page number for pagination

**Response (200 OK):**
```json
{
    "count": 100,
    "next": "http://localhost:8000/api/social/posts/?page=2",
    "previous": null,
    "results": [
        {
            "id": "uuid-here",
            "author": {
                "id": 1,
                "username": "john_doe",
                "first_name": "John",
                "last_name": "Doe",
                "profile_picture": "/media/profile_pictures/john.jpg"
            },
            "content": "Check out this amazing post! #awesome #socialmedia",
            "post_type": "text",
            "visibility": "public",
            "media_files": [],
            "link_url": null,
            "link_title": null,
            "link_description": null,
            "link_image": null,
            "view_count": 150,
            "share_count": 5,
            "comment_count": 12,
            "like_count": 25,
            "hashtags": ["awesome", "socialmedia"],
            "mentions": [],
            "is_edited": false,
            "edited_at": null,
            "is_pinned": false,
            "is_featured": false,
            "is_liked_by_user": true,
            "is_shared_by_user": false,
            "is_bookmarked_by_user": true,
            "comments": [...],
            "created_at": "2024-01-01T12:00:00Z",
            "updated_at": "2024-01-01T12:00:00Z"
        }
    ]
}
```

#### 1.2 Create Post
**POST** `/posts/`

Create a new post.

**Request Body:**
```json
{
    "content": "This is my first post! #firstpost #excited",
    "post_type": "text",
    "visibility": "public",
    "media_files": [],
    "link_url": null,
    "link_title": null,
    "link_description": null,
    "link_image": null
}
```

**Response (201 Created):**
```json
{
    "id": "uuid-here",
    "author": {
        "id": 1,
        "username": "john_doe",
        "first_name": "John",
        "last_name": "Doe",
        "profile_picture": "/media/profile_pictures/john.jpg"
    },
    "content": "This is my first post! #firstpost #excited",
    "post_type": "text",
    "visibility": "public",
    "media_files": [],
    "link_url": null,
    "link_title": null,
    "link_description": null,
    "link_image": null,
    "view_count": 0,
    "share_count": 0,
    "comment_count": 0,
    "like_count": 0,
    "hashtags": ["firstpost", "excited"],
    "mentions": [],
    "is_edited": false,
    "edited_at": null,
    "is_pinned": false,
    "is_featured": false,
    "is_liked_by_user": false,
    "is_shared_by_user": false,
    "is_bookmarked_by_user": false,
    "comments": [],
    "created_at": "2024-01-01T12:00:00Z",
    "updated_at": "2024-01-01T12:00:00Z"
}
```

#### 1.3 Get Post Details
**GET** `/posts/{id}/`

Get detailed information about a specific post.

**Response (200 OK):**
```json
{
    "id": "uuid-here",
    "author": {...},
    "content": "Post content here",
    "post_type": "text",
    "visibility": "public",
    "media_files": [],
    "link_url": null,
    "link_title": null,
    "link_description": null,
    "link_image": null,
    "view_count": 150,
    "share_count": 5,
    "comment_count": 12,
    "like_count": 25,
    "hashtags": ["awesome", "socialmedia"],
    "mentions": [],
    "is_edited": false,
    "edited_at": null,
    "is_pinned": false,
    "is_featured": false,
    "is_liked_by_user": true,
    "is_shared_by_user": false,
    "is_bookmarked_by_user": true,
    "comments": [...],
    "created_at": "2024-01-01T12:00:00Z",
    "updated_at": "2024-01-01T12:00:00Z"
}
```

#### 1.4 Update Post
**PUT/PATCH** `/posts/{id}/`

Update a post (owner only).

**Request Body:**
```json
{
    "content": "Updated post content! #updated #socialmedia",
    "visibility": "public",
    "media_files": []
}
```

**Response (200 OK):**
```json
{
    "id": "uuid-here",
    "content": "Updated post content! #updated #socialmedia",
    "hashtags": ["updated", "socialmedia"],
    "mentions": [],
    "is_edited": true,
    "edited_at": "2024-01-01T13:00:00Z",
    ...
}
```

#### 1.5 Delete Post
**DELETE** `/posts/{id}/`

Delete a post (owner only).

**Response (204 No Content):**
No content returned.

#### 1.6 Like/Unlike Post
**POST** `/posts/{id}/like/`

Like or unlike a post.

**Response (200 OK):**
```json
{
    "action": "liked",
    "like_count": 26
}
```

#### 1.7 Share Post
**POST** `/posts/{id}/share/`

Share or unshare a post.

**Response (200 OK):**
```json
{
    "action": "shared",
    "share_count": 6
}
```

#### 1.8 Bookmark Post
**POST** `/posts/{id}/bookmark/`

Bookmark or unbookmark a post.

**Response (200 OK):**
```json
{
    "action": "bookmarked"
}
```

#### 1.9 Track Post View
**POST** `/posts/{id}/view/`

Track a post view.

**Response (200 OK):**
```json
{
    "view_count": 151
}
```

#### 1.10 Get Post Analytics
**GET** `/posts/{id}/analytics/`

Get detailed analytics for a post.

**Response (200 OK):**
```json
{
    "id": "uuid-here",
    "view_count": 150,
    "like_count": 25,
    "share_count": 5,
    "comment_count": 12,
    "engagement_breakdown": {
        "view": 150,
        "like": 25,
        "share": 5,
        "comment": 12,
        "bookmark": 8
    },
    "created_at": "2024-01-01T12:00:00Z"
}
```

#### 1.11 Get My Posts
**GET** `/posts/my_posts/`

Get current user's posts.

**Response (200 OK):**
```json
{
    "count": 25,
    "next": null,
    "previous": null,
    "results": [...]
}
```

#### 1.12 Get Liked Posts
**GET** `/posts/liked_posts/`

Get posts liked by current user.

**Response (200 OK):**
```json
{
    "count": 15,
    "next": null,
    "previous": null,
    "results": [...]
}
```

#### 1.13 Get Bookmarked Posts
**GET** `/posts/bookmarked_posts/`

Get posts bookmarked by current user.

**Response (200 OK):**
```json
{
    "count": 8,
    "next": null,
    "previous": null,
    "results": [...]
}
```

#### 1.14 Get Trending Posts
**GET** `/posts/trending/`

Get trending posts (high engagement in last 24 hours).

**Response (200 OK):**
```json
[
    {
        "id": "uuid-here",
        "content": "Trending post content",
        "engagement_score": 150,
        ...
    }
]
```

### 2. Comment Management

#### 2.1 List Comments
**GET** `/comments/`

Get comments with filtering and pagination.

**Query Parameters:**
- `post`: Filter by post ID
- `author`: Filter by author ID
- `parent_comment`: Filter by parent comment ID
- `ordering`: Sort by created_at, like_count
- `page`: Page number for pagination

**Response (200 OK):**
```json
{
    "count": 50,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": "uuid-here",
            "post": "post-uuid",
            "author": {
                "id": 2,
                "username": "jane_doe",
                "first_name": "Jane",
                "last_name": "Doe",
                "profile_picture": "/media/profile_pictures/jane.jpg"
            },
            "parent_comment": null,
            "content": "Great post! Thanks for sharing.",
            "media_files": [],
            "like_count": 5,
            "mentions": [],
            "is_edited": false,
            "edited_at": null,
            "replies": [...],
            "is_liked_by_user": false,
            "created_at": "2024-01-01T12:30:00Z",
            "updated_at": "2024-01-01T12:30:00Z"
        }
    ]
}
```

#### 2.2 Create Comment
**POST** `/comments/`

Create a new comment.

**Request Body:**
```json
{
    "post": "post-uuid",
    "parent_comment": null,
    "content": "This is a great comment!",
    "media_files": []
}
```

**Response (201 Created):**
```json
{
    "id": "uuid-here",
    "post": "post-uuid",
    "author": {...},
    "parent_comment": null,
    "content": "This is a great comment!",
    "media_files": [],
    "like_count": 0,
    "mentions": [],
    "is_edited": false,
    "edited_at": null,
    "replies": [],
    "is_liked_by_user": false,
    "created_at": "2024-01-01T12:30:00Z",
    "updated_at": "2024-01-01T12:30:00Z"
}
```

#### 2.3 Get Comment Details
**GET** `/comments/{id}/`

Get detailed information about a specific comment.

**Response (200 OK):**
```json
{
    "id": "uuid-here",
    "post": "post-uuid",
    "author": {...},
    "parent_comment": null,
    "content": "Comment content",
    "media_files": [],
    "like_count": 5,
    "mentions": [],
    "is_edited": false,
    "edited_at": null,
    "replies": [...],
    "is_liked_by_user": false,
    "created_at": "2024-01-01T12:30:00Z",
    "updated_at": "2024-01-01T12:30:00Z"
}
```

#### 2.4 Update Comment
**PUT/PATCH** `/comments/{id}/`

Update a comment (owner only).

**Request Body:**
```json
{
    "content": "Updated comment content"
}
```

**Response (200 OK):**
```json
{
    "id": "uuid-here",
    "content": "Updated comment content",
    "is_edited": true,
    "edited_at": "2024-01-01T13:00:00Z",
    ...
}
```

#### 2.5 Delete Comment
**DELETE** `/comments/{id}/`

Delete a comment (owner only).

**Response (204 No Content):**
No content returned.

#### 2.6 Like/Unlike Comment
**POST** `/comments/{id}/like/`

Like or unlike a comment.

**Response (200 OK):**
```json
{
    "action": "liked",
    "like_count": 6
}
```

#### 2.7 Get My Comments
**GET** `/comments/my_comments/`

Get current user's comments.

**Response (200 OK):**
```json
{
    "count": 20,
    "next": null,
    "previous": null,
    "results": [...]
}
```

### 3. Engagement Tracking

#### 3.1 Track Engagement
**POST** `/engagements/`

Track a new engagement.

**Request Body:**
```json
{
    "post": "post-uuid",
    "engagement_type": "view",
    "metadata": {
        "source": "homepage",
        "device": "mobile"
    }
}
```

**Response (201 Created):**
```json
{
    "id": "uuid-here",
    "post": "post-uuid",
    "user": {
        "id": 1,
        "username": "john_doe",
        "first_name": "John",
        "last_name": "Doe",
        "profile_picture": "/media/profile_pictures/john.jpg"
    },
    "engagement_type": "view",
    "metadata": {
        "source": "homepage",
        "device": "mobile"
    },
    "ip_address": "192.168.1.1",
    "user_agent": "Mozilla/5.0...",
    "created_at": "2024-01-01T12:00:00Z"
}
```

#### 3.2 Get My Engagements
**GET** `/engagements/my_engagements/`

Get current user's engagements.

**Response (200 OK):**
```json
{
    "count": 100,
    "next": null,
    "previous": null,
    "results": [...]
}
```

#### 3.3 Get Engagement Analytics (Admin Only)
**GET** `/engagements/analytics/`

Get engagement analytics for the platform.

**Query Parameters:**
- `days`: Number of days to analyze (default: 7)

**Response (200 OK):**
```json
{
    "engagement_breakdown": [
        {
            "engagement_type": "view",
            "count": 1500
        },
        {
            "engagement_type": "like",
            "count": 250
        },
        {
            "engagement_type": "comment",
            "count": 100
        }
    ],
    "daily_trend": [
        {
            "date": "2024-01-01",
            "count": 200
        },
        {
            "date": "2024-01-02",
            "count": 180
        }
    ],
    "total_engagements": 1850
}
```

### 4. Hashtag Management

#### 4.1 List Hashtags
**GET** `/hashtags/`

Get all hashtags with filtering and search.

**Query Parameters:**
- `is_trending`: Filter by trending status
- `search`: Search by hashtag name
- `ordering`: Sort by post_count, trend_score, created_at
- `page`: Page number for pagination

**Response (200 OK):**
```json
{
    "count": 50,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": 1,
            "name": "awesome",
            "post_count": 150,
            "is_trending": true,
            "trend_score": 85.5,
            "created_at": "2024-01-01T00:00:00Z",
            "updated_at": "2024-01-01T12:00:00Z"
        }
    ]
}
```

#### 4.2 Get Hashtag Details
**GET** `/hashtags/{id}/`

Get detailed information about a specific hashtag.

**Response (200 OK):**
```json
{
    "id": 1,
    "name": "awesome",
    "post_count": 150,
    "is_trending": true,
    "trend_score": 85.5,
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T12:00:00Z"
}
```

#### 4.3 Get Trending Hashtags
**GET** `/hashtags/trending/`

Get trending hashtags.

**Response (200 OK):**
```json
[
    {
        "id": 1,
        "name": "awesome",
        "post_count": 150,
        "trend_score": 85.5,
        "is_trending": true
    },
    {
        "id": 2,
        "name": "socialmedia",
        "post_count": 120,
        "trend_score": 75.2,
        "is_trending": true
    }
]
```

#### 4.4 Get Posts by Hashtag
**GET** `/hashtags/{id}/posts/`

Get posts for a specific hashtag.

**Response (200 OK):**
```json
{
    "count": 25,
    "next": null,
    "previous": null,
    "results": [...]
}
```

### 5. Media File Management

#### 5.1 Upload Media File
**POST** `/media/`

Upload a media file.

**Request Body (multipart/form-data):**
```
file: <file>
file_type: image
```

**Response (201 Created):**
```json
{
    "id": "uuid-here",
    "file": "/media/social_media/image.jpg",
    "file_type": "image",
    "file_name": "image.jpg",
    "file_size": 1024000,
    "mime_type": "image/jpeg",
    "width": 1920,
    "height": 1080,
    "duration": null,
    "created_at": "2024-01-01T12:00:00Z"
}
```

#### 5.2 Get Media File Details
**GET** `/media/{id}/`

Get detailed information about a media file.

**Response (200 OK):**
```json
{
    "id": "uuid-here",
    "file": "/media/social_media/image.jpg",
    "file_type": "image",
    "file_name": "image.jpg",
    "file_size": 1024000,
    "mime_type": "image/jpeg",
    "width": 1920,
    "height": 1080,
    "duration": null,
    "created_at": "2024-01-01T12:00:00Z"
}
```

#### 5.3 Delete Media File
**DELETE** `/media/{id}/`

Delete a media file (owner only).

**Response (204 No Content):**
No content returned.

## Error Responses

### 400 Bad Request
```json
{
    "content": [
        "Post content cannot be empty."
    ],
    "post_type": [
        "Link URL is required for link posts."
    ]
}
```

### 401 Unauthorized
```json
{
    "detail": "Authentication credentials were not provided."
}
```

### 403 Forbidden
```json
{
    "detail": "You do not have permission to perform this action."
}
```

### 404 Not Found
```json
{
    "detail": "Not found."
}
```

### 429 Too Many Requests
```json
{
    "detail": "Request was throttled. Expected available in 60 seconds."
}
```

## Rate Limiting

- **General API**: 200 requests per hour per user
- **File Uploads**: 50 uploads per hour per user

## Pagination

All list endpoints support pagination with the following parameters:
- `page`: Page number (default: 1)
- `page_size`: Items per page (default: 20, max: 100)

## Filtering and Search

### Post Endpoints
- **Search**: content, hashtags, mentions
- **Filter**: post_type, visibility, author, is_pinned, is_featured
- **Order**: created_at, updated_at, like_count, comment_count, view_count

### Comment Endpoints
- **Filter**: post, author, parent_comment
- **Order**: created_at, like_count

### Engagement Endpoints
- **Filter**: post, user, engagement_type
- **Order**: created_at

### Hashtag Endpoints
- **Search**: name
- **Filter**: is_trending
- **Order**: post_count, trend_score, created_at

## File Upload

For media file uploads, use `multipart/form-data` content type:

```
Content-Type: multipart/form-data

{
    "file": <file>,
    "file_type": "image"
}
```

**Supported File Types:**
- **Images**: JPEG, PNG, GIF, WebP
- **Videos**: MP4, AVI, MOV, WebM
- **Audio**: MP3, WAV, OGG
- **Documents**: PDF, DOC, DOCX, TXT

**File Size Limits:**
- **Images**: 10MB
- **Videos**: 100MB
- **Audio**: 50MB
- **Documents**: 25MB

## Security Considerations

1. **Authentication**: All endpoints require JWT authentication
2. **Authorization**: Users can only modify their own content
3. **Rate Limiting**: Prevents abuse and spam
4. **File Validation**: All uploaded files are validated for type and size
5. **Content Filtering**: Hashtags and mentions are automatically extracted
6. **Privacy**: Posts respect visibility settings (public, private, followers)

## Testing

You can test the APIs using tools like:
- **Postman**
- **cURL**
- **Insomnia**
- **Django REST Framework's browsable API**

Example cURL command for creating a post:
```bash
curl -X POST http://localhost:8000/api/social/posts/ \
  -H "Authorization: Bearer <your_jwt_token>" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Hello world! #firstpost #excited",
    "post_type": "text",
    "visibility": "public"
  }'
```

## LinkedIn Integration

### Test LinkedIn Connection
**GET** `/linkedin/test-connection/`

Test if the user's LinkedIn account is connected and retrieve account info.

**Response (200 OK):**
```json
{
    "status": "success",
    "message": "LinkedIn account is connected",
    "account_info": {"id": "abc123", "name": "John Doe"}
}
```

### Post to LinkedIn
**POST** `/linkedin/post/`

Create a new post on the user's LinkedIn account.

**Request Body:**
```json
{
    "content": "This is a LinkedIn post! #networking"
}
```

**Response (201 Created):**
```json
{
    "status": "success",
    "message": "Post published successfully",
    "post_id": "urn:li:share:123456789"
}
```

### Get LinkedIn User Info
**GET** `/linkedin/userinfo/`

Fetches the authenticated user's LinkedIn profile info using the /userinfo endpoint.

**Response (200 OK):**
```json
{
    "status": "success",
    "userinfo": { /* LinkedIn user info object */ }
}
```

### Post to LinkedIn with Image
**POST** `/linkedin/post-image/`

Create a new post on the user's LinkedIn account with an image.

**Request:**
- Content-Type: multipart/form-data
- Fields:
  - `content`: The post text
  - `image`: The image file to upload

**Response (201 Created):**
```json
{
    "status": "success",
    "message": "Post with image published successfully",
    "post_id": "urn:li:share:123456789",
    "asset_urn": "urn:li:digitalmediaAsset:abc123"
}
```
