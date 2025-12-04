---
title: "REST API Overview"
weight: 1
---

## REST APIs

A REST API (Representational State Transfer Application Programming Interface) is an architectural style for designing networked applications. It's a set of principles and constraints that guide how web services communicate over the internet, primarily using the HTTP protocol. 

## API: Social Platform User Management

### Why I Built This REST API

I built this REST API because I wanted to implement a medium build API that also included security features. This API could have endpoints for user management, authentication, user profile management, and social interactions (like friendships). This scenario provides a good mixture of security (login, profile management), relationship (friends), and CRUD operations. 

### What This REST API Solves

This REST API simulates a social networking backend. It allows users to create accounts, authenticate securely, manage their profiles, send and accept friend requests, and manage friendships.

---

# Endpoints

## User Endpoints

| Method | Endpoint              | Description | Auth Required | Access |
|--------|------------------------|-------------|---------------|--------|
| POST   | /api/users/register   | Registers a new user. | No  | Public |
| GET    | /api/users            | Lists all users. | Yes | Admin |
| DELETE | /api/users/me         | Deletes the current user. | Yes | User |
| POST   | /api/users/me/password | Updates user password. | Yes | User |
| DELETE | /api/users/{id}       | Deletes any user by ID. | Yes | Admin |

---

## Friend Endpoints

| Method | Endpoint | Description | Auth | Access |
|--------|----------|-------------|------|--------|
| GET    | /api/friends | Lists all friends of logged-in user | Yes | User |
| GET    | /api/friends/requests | Lists pending friend requests | Yes | User |
| POST   | /api/friends/request/{receiverId} | Sends friend request | Yes | User |
| POST   | /api/friends/accept/{requestId} | Accepts friend request | Yes | User |
| POST   | /api/friends/reject/{requestId} | Rejects friend request | Yes | User |
| DELETE | /api/friends/{friendId} | Removes a friend | Yes | User |

---

## Number of APIs Implemented

I implemented 10 REST APIs. For a medium-level build with 10 endpoints, I wanted to create a well-structured API that covers basic CRUD (Create, Read, Update, Delete) operations, authentication, and some relational logic.

This would typically involve user management or social interactions. A good balance of complexity would be a user-focused API with some added functionality like friendships, authentication, and profile management.
