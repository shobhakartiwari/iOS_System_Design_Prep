# iOS System Design Interview Prep: Photo-Sharing App

This README outlines a high-level system design approach for an iOS-based photo-sharing app, tailored for iOS system design interview prep. It breaks down functional and non-functional requirements, provides an iOS-centric architecture, and highlights key considerations like scalability, caching, and accessibility. Perfect for practicing how to think through system design questions with an iOS twist!

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
