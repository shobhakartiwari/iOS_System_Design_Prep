# iOS System Design Interview Prep: Photo-Sharing App 

This README outlines a high-level system design approach for an iOS-based photo-sharing app, tailored for iOS system design interview prep. It breaks down functional and non-functional requirements, provides an iOS-centric architecture, and highlights key considerations like scalability, caching, and accessibility. Perfect for practicing how to think through system design questions with an iOS twist!

- This repository created and managed by [Shobhakar Tiwari](https://topmate.io/shobhakartiwari) 
---

## Table of Contents
1. [Problem Statement](#problem-statement)
2. [Requirements](#requirements)
   - [Functional Requirements](#functional-requirements)
   - [Non-Functional Requirements](#non-functional-requirements)
3. [High-Level System Design](#high-level-system-design)
4. [iOS Implementation Details](#ios-implementation-details)
   - [Upload Photos](#upload-photos)
   - [Feed](#feed)
   - [Likes/Comments](#likescomments)
   - [User Profiles](#user-profiles)
5. [Non-Functional Considerations](#non-functional-considerations)
   - [Scalability](#scalability)
   - [Performance](#performance)
   - [Accessibility](#accessibility)
   - [Offline Support](#offline-support)
6. [Summary](#summary)

---

## Problem Statement
Design a photo-sharing app for iOS where users can upload photos, view a feed of posts from followed users, like/comment on photos, and check out user profiles. The app should scale to handle up to a million users while maintaining a smooth, accessible, and offline-friendly experience.

---

## Requirements
Let’s Start Fresh: Understand the Problem

Imagine the interviewer says, “Design a photo-sharing app for iOS.” First thing I’d do is slow down and clarify what we’re building. I’d ask: “Are we talking Instagram vibes—uploading, feeds, likes? Any specific features like stories or filters? And who’s using it—hundreds of users or millions?” Let’s assume they say: “Users upload photos, see a feed, like/comment, and we’re targeting a decent-sized audience, maybe a million users eventually.”

Cool, so now we’ve got a rough idea. Let’s break it into functional and non-functional requirements like you said—it’s a solid way to keep things organized.

### Functional Requirements
- **Upload Photos**: Users can select or capture photos and share them.
- **Feed**: Displays a scrollable list of photos from followed users, ordered by recency.
- **Likes/Comments**: Users can like and comment on photos, with real-time(ish) updates.
- **User Profiles**: View a user’s posts and basic info.

### Non-Functional Requirements
- **Performance**: Fast feed loading and responsive interactions.
- **Scalability**: Supports up to 1 million users without degradation.
- **Reliability**: Consistent data and reliable uploads.
- **Accessibility**: Usable with VoiceOver, Dynamic Type, etc.
- **Offline Support**: View cached photos and queue uploads offline.
- **Privacy**: Secure photo access and permission handling.

---

## High-Level System Design
The app consists of:
- **iOS Client**: Handles UI, local storage, and network requests.
- **Backend**: Manages photo storage, user data, and feed generation (assumed simple for this exercise).
- **Network**: RESTful API connects the client and backend.


---

## iOS Implementation Details

### Upload Photos
- **Flow**: User picks a photo or snaps one, app compresses it, and sends it to the backend.
- **iOS Tech**:
  - `PHPicker` or `UIImagePickerController` for selection.
  - `AVFoundation`/`Core Image` for compression (e.g., max 1MB).
  - `URLSession` with async/await for background uploads.
- **Scale**: Queue offline uploads via `OperationQueue`.
- **Accessibility**: VoiceOver labels for buttons.

### Feed
- **Flow**: Scrollable list of photos from followed users, paginated.
- **iOS Tech**:
  - SwiftUI `LazyVStack` (or UIKit `UICollectionView`) for efficient scrolling.
  - REST API: `GET /feed?page=1&limit=20`.
  - Cache thumbnails in `NSCache` or Core Data.
- **Scale**: Paginate API; limit cache size (e.g., 100MB).
- **Accessibility**: Alt text for images, Dynamic Type support.

### Likes/Comments
- **Flow**: Tap to like, type to comment, see updates.
- **iOS Tech**:
  - API: `POST /photo/{id}/like`, `POST /photo/{id}/comment`.
  - Optimistic UI updates with Combine for reactivity.
- **Scale**: Batch offline updates; debounce rapid actions.
- **Accessibility**: Announce actions via VoiceOver.

### User Profiles
- **Flow**: View a user’s photo grid and bio.
- **iOS Tech**:
  - SwiftUI `View` or UIKit `UIViewController`.
  - API: `GET /user/{id}/posts`; cache locally.
- **Scale**: Preload frequent profiles.
- **Accessibility**: Clear headings, readable fonts.

---

## Non-Functional Considerations

### Scalability
- **User Head Count**: For 1M users, paginate feeds, cache efficiently, and offload resizing to the backend.
- **Caching**: `NSCache` for thumbnails; Core Data/Filesystem for persistence with LRU eviction.

### Performance
- Lazy-load images (`AsyncImage` or equivalent).
- Use `BGTaskScheduler` for background syncs, avoiding main thread bottlenecks.

### Accessibility
- Leverage iOS features: VoiceOver, Dynamic Type, high-contrast mode.
- Test with real devices for authentic feedback.

### Offline Support
- Cache recent feed in Core Data.
- Queue uploads with `BackgroundTasks` for later sync.

---

## Summary
This design delivers a photo-sharing iOS app with a SwiftUI-driven UI, `URLSession` for networking, and Core Data for caching. It scales to 1M users via pagination and smart caching, ensures accessibility with VoiceOver, and supports offline use. The backend (lightly touched here) would use S3 for photo storage and a database for metadata. This approach balances iOS-specific implementation with system-level thinking—great for interview practice!

---
