# Conference Room Booking Micro-Service
This API allows you to manage and book conference rooms in your organization. It provides endpoints to check room availability, fetch room details, book a room, and retrieve bookings within a specific time slot.

## Table of Contents
- [Database](#database)
- [Maintenance Timings](#maintenance-timings)
- [API Endpoints](#api-endpoints)
  1. [Fetch Available Conference Room (POST)](#1-fetch-available-conference-room-post)
  2. [Fetch Room Details by Name (GET)](#2-fetch-room-details-by-name-get)
  3. [Book a Room (POST)](#3-book-a-room-post)
  4. [Fetch All Bookings within a Slot (POST)](#4-fetch-all-bookings-within-a-slot-post)

## Database
Two tables are created `Room` and `Booking`

&nbsp;`Room` table has the room details like below

  | Room Id | Name    | Capacity   |
  |---------|---------|------------|
  | 1       | Amaze   | 3 persons  |
  | 2       | Beauty  | 7 persons  |
  | 3       | Inspire | 12 persons |
  | 4       | Strive  | 20 persons |

&nbsp;One sample booking has been added into `Booking` table on application start up for test.

`2024-01-17` here is `current date`

| Booking Id | Room Id | Start_Time          | End_Time            | Number_Of_People |
|------------|---------|---------------------|---------------------|------------------|
| 1          | 3       | 2024-01-17 10:00:00 | 2024-01-17 11:00:00 | 5                |

## Maintenance Timings
Maintenance Timings of the room are configurable from the `application.yml` file

```yml
maintenance:
  timings:
  - startTime: "09:00"
    endTime: "09:15"
  - startTime: "13:00"
    endTime: "13:15"
  - startTime: "17:00"
    endTime: "17:15"
```

## API Endpoints
### 1. Fetch Available Conference Room (POST)
> **Definition**: Fetch available conference rooms.

**Endpoint**: `/api/conference-room`

**Body**:

```json
{
  "timeRange": {
    "startTime": "2024-01-17 17:21:00",
    "endTime": "2024-01-17 17:30:00"
  },
  "numberOfPeople": 11
}
```
Response:

```json
{
  "status": "success",
  "data": {
    "availableRooms": [
      "Inspire",
      "Strive"
    ]
  }
}
```
Sample Error Response:

```json
{
  "status": "error",
  "errorCode": "Bad Request",
  "errorMessage": "Entered Time is not suitable input"
}
```
### 2. Fetch Room Details by Name (GET)
   Definition: Fetch room details by name.

Endpoint: `/api/conference-room?name=Inspire`

Response:

```json
{
  "status": "success",
  "data": {
    "roomId": 3,
    "name": "Inspire",
    "capacity": 12,
    "maintenanceTimings": [
      {
        "startTime": "09:00",
        "endTime": "09:15"
      },
      {
        "startTime": "13:00",
        "endTime": "13:15"
      },
      {
        "startTime": "17:00",
        "endTime": "17:15"
      }
    ]
  }
}
```
Sample Error Response:

```json
{
  "status": "error",
  "errorCode": "NO_SUCH_ROOM",
  "errorMessage": "No room exists with this name or id"
}
```
### 3. Book a Room (POST)
   Definition: Book a conference room.

Endpoint: `/api/conference-bookings/book`

Body:

```json
{
  "roomId": 3,
  "timeRange": {
    "startTime": "2024-01-17 18:11:00",
    "endTime": "2024-01-17 19:45:00"
  },
  "numberOfPeople": 13
}
```
Response:

```json
{
  "status": "success",
  "data": "Conference Room Booked"
}
```
Sample Error Response:

```json
{
  "status": "error",
  "errorCode": "Bad Request",
  "errorMessage": "Entered Time is not suitable input"
}
```
### 4. Fetch All Bookings within a Slot (POST)
   Definition: Fetch all bookings within a time slot.

Endpoint: `/api/conference-bookings`

Body:

```json
{
  "startTime": "2024-01-17 17:00:00",
  "endTime": "2024-01-17 19:30:00"
}
```
Response:

```json
[
  {
    "roomId": 4,
    "timeRange": {
      "startTime": "2024-01-17 17:30:00",
      "endTime": "2024-01-17 17:45:00"
    },
    "numberOfPeople": 13
  },
  {
    "roomId": 4,
    "timeRange": {
      "startTime": "2024-01-17 18:30:00",
      "endTime": "2024-01-17 19:45:00"
    },
    "numberOfPeople": 13
  }
]
```
Sample Error Response:

```json
{
  "status": "error",
  "errorCode": "Bad Request",
  "errorMessage": "Entered Time is not suitable input"
}
```