# Events Management API

This is a Laravel-based API for managing events, providing CRUD operations, email notifications, security features, and
more.

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Getting Started](#getting-started)
    - [Docker Setup](#docker-setup)
    - [Configuration](#configuration)
    - [Running the Application](#running-the-application)
- [API Endpoints](#api-endpoints)
    - [Authentication](#authentication)
    - [Query Parameters and Relationships](#query-parameters-and-relationships)
- [Queues and Email Notifications](#queues-and-email-notifications)
- [Security Practices](#security-practices)

## Features

- CRUD operations for managing events.
- Email notifications for event reminders sent 24 hours before the event.
- Docker setup for easy development and deployment.
- Policies and security measures for data protection.
- Throttling to prevent abuse and ensure API stability.
- Utilization of Laravel Traits for code organization.

## Requirements

- [Docker](https://www.docker.com/) and [Docker Compose](https://docs.docker.com/compose/)
- PHP 8.0 and above
- [Postman](https://web.postman.co/) or [Insomnia](https://insomnia.rest/)

## Getting Started

### Docker Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/Victoria-ElenaLazar/Events_Management/tree/main
2. Navigate to the project directory:
   ```bash
   cd event-management
3. Create a copy of the .env.example file:
   ```bash
   cp .env.example .env
4. Configure the .env file with your database and email settings.

## Configuration

1. Install dependencies:
   ```bash
   composer install
2. Generate application key:
   ```bash
   php artisan key:generate

3. Run Database Migration:
   ```bash
   php artisan migrate

## Run the application

In your project directory, inside the terminal, write the following commands:
   ````
   docker-compose up -d
   php artisan serve
   ````

Open the http://localhost in a testing API tool, such as [Postman](https://web.postman.co/)
or [Insomnia](https://insomnia.rest/)

## API Endpoints

**1. Get a list with all events**
   ````
   GET /Api/events
   ````

**Response:**
   ````
   {
	"data": [
		{
			"id": 201,
			"name": "Event Name",
			"description": "Event Description",
			"start_time": "2023-01-01 10:00:00",
			"end_time": "2023-01-01 12:00:00"
		},
   ````
**2. Create a new event**
   ````
   POST /Api/events

   ````
**Request:**
   ````
{
    "name": "Event Name comes here",
    "description": "Event Description",
    "start_time": "2023-01-01 10:00:00",
    "end_time": "2023-01-01 12:00:00"
}
   ````

**Response:**

   ````
   {
	"data": {
		"id": 202,
		"name": "Event Name comes here",
		"description": "Event Description",
		"start_time": "2023-01-01 10:00:00",
		"end_time": "2023-01-01 12:00:00"
	}
}
   ````

### Authentication
The API supports token-based authentication. Users can obtain an access token by logging in, and they can log out to invalidate the token.

**1. Login**
   ````
   POST /Api/login
   ````
**Request:**
   ````
   {
  "email": "user@example.com",
  "password": "userpassword"
}
   ````
**Response:**

   ````
   {
	"token": "7|gN3aWlpSW0IXigu0SRBSVIenig4V2RA22bJ4qCZZ7779e3cc"
}
   ````

### Query Parameters and Relationships
The API supports query parameters for including related data in the responses.

Supported Parameters
include: Use the include parameter to specify related resources. You can include multiple relationships using a comma-separated list. For example:
   ````
  GET /Api/events?include=user,attendee.user,attendees
   ````
**Response:**
   ````
   {
	"data": [
		{
			"id": 70,
			"name": "Eum eaque consequatur.",
			"description": "Dolores ab magni facere. Facere consequuntur dolorem facere unde eum. Quam nemo similique eum voluptas aliquam est. Consequuntur dolorem quae et cum.",
			"start_time": "2023-12-31 18:14:26",
			"end_time": "2024-01-30 06:57:43",
			"user": {
				"id": 958,
				"name": "Brennan Murphy MD",
				"email": "giovanni91@example.net",
				"email_verified_at": "2023-12-12T09:53:05.000000Z",
				"created_at": "2023-12-12T09:53:10.000000Z",
				"updated_at": "2023-12-12T09:53:10.000000Z"
			},
			"attendees": [
				{
					"id": 20,
					"user_id": 12,
					"event_id": 70,
					"created_at": "2023-12-12T09:53:12.000000Z",
					"updated_at": "2023-12-12T09:53:12.000000Z"
				},
				{
					"id": 26,
					"user_id": 15,
					"event_id": 70,
					"created_at": "2023-12-12T09:53:12.000000Z",
					"updated_at": "2023-12-12T09:53:12.000000Z"
				},
				{
					"id": 34,
					"user_id": 19,
					"event_id": 70,
					"created_at": "2023-12-12T09:53:12.000000Z",
					"updated_at": "2023-12-12T09:53:12.000000Z"
				},
				{
					"id": 72,
					"user_id": 39,
					"event_id": 70,
					"created_at": "2023-12-12T09:53:12.000000Z",
					"updated_at": "2023-12-12T09:53:12.000000Z"
				},
				{
					"id": 144,
					"user_id": 73,
					"event_id": 70,
					"created_at": "2023-12-12T09:53:12.000000Z",
					"updated_at": "2023-12-12T09:53:12.000000Z"
				},
				{
					"id": 624,
					"user_id": 314,
					"event_id": 70,
					"created_at": "2023-12-12T09:53:15.000000Z",
					"updated_at": "2023-12-12T09:53:15.000000Z"
				},

   ````

## Queues and Email Notifications
To handle email notifications, the application utilizes Laravel queues. Follow these steps to set up and run the necessary workers:

**1. Create queue table:**
   ````
   php artisan queue:table
   ````

**2. Run Migrations:**
   ````
   php artisan migrate
   ````

**3. Send Event Reminders**
   ````
   php artisan app:send-event-reminders
   ````
**4. Start Queue Worker:**
   ````
   app php artisan queue:work
   ````
This will continuously process jobs from the queue.

Additionally, for email configuration, update your .env file with the appropriate mail settings, and make sure to configure config/mail.php. Set the mailer to Mailhog for local testing.

## Security Practices
The application implements various security measures to protect user data and ensure stable functionality:

**Throttling:** Implemented to prevent abuse and ensure API stability.

**User Permissions:** Users can only create and delete events they have created.

**Attendee Permissions:** Attendees can only unattend events they are attending.

**Authentication and Authorization:** Token-based authentication and authorization for protected routes.

**Input Validation:** Ensure proper input validation to protect against malicious data.
