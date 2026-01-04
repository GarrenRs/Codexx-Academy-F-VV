# Codexx Academy

## Overview

Codexx Academy is a premium professional portfolio platform designed for high-caliber professionals. The platform centers on a "Proof-of-Work" philosophy, emphasizing verified execution history over self-proclaimed skills. It provides multi-tenant workspace functionality where professionals can showcase their portfolios, manage clients, and communicate through internal messaging systems.

The application is a full-stack Flask web application with PostgreSQL database, featuring role-based access control (Admin/Demo/User), dual-channel notifications (Telegram/SMTP), and a themeable frontend with multiple visual styles.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Backend Framework
- **Flask 3.x with Python 3.11**: Chosen for rapid development and extensive ecosystem. The application uses a monolithic architecture with all routes defined in `app.py`.
- **SQLAlchemy ORM**: Database abstraction layer with Flask-SQLAlchemy integration for model definitions and queries.
- **Session-based Authentication**: Custom implementation with role differentiation (Admin, Demo User, Regular User) stored in Flask sessions.

### Database Design
- **PostgreSQL**: Primary database with connection pooling enabled (pool_size=10, pool_recycle=3600).
- **Multi-tenant Model**: Workspace-based architecture where each user belongs to a workspace, and all content (projects, skills, clients, messages) is scoped to workspaces.
- **Key Models**: Workspace, User, Project, Skill, Client, Message, VisitorLog
- **JSONB columns**: Used for flexible data like badges and technologies arrays.
- **Fallback to SQLite**: When PostgreSQL is unavailable, the system falls back to SQLite for local development.

### Frontend Architecture
- **Jinja2 Templates**: Server-side rendering with template inheritance (base templates for public and dashboard views).
- **Bootstrap 5**: CSS framework for responsive layout and components.
- **Theme System**: Multiple CSS theme files (luxury-gold, modern-dark, clean-light, etc.) that can be switched per user preference using CSS custom properties.
- **Dashboard vs Public**: Separate template hierarchies for admin dashboard (`templates/dashboard/`) and public-facing portfolio pages.

### Authentication & Authorization
- **Three-tier Role System**: 
  - Admin: Full system access, user management, platform-wide messaging
  - Regular User: Portfolio management, client tracking, internal messaging
  - Demo User: Restricted access for showcasing platform features
- **Password Hashing**: Werkzeug's scrypt-based password hashing
- **Session Management**: 7-day session lifetime with secure cookie settings

### Background Processing
- **APScheduler**: Used for automated backup jobs and scheduled tasks
- **Backup System**: JSON-based backups stored in `backups/` directory with metadata tracking

### Security Features
- **IP Activity Logging**: Failed login attempts and security events logged to `security/ip_log.json`
- **Rate Limiting**: Protection against brute force attacks
- **Separate Notification Channels**: Admin and user notifications are isolated for privacy

## External Dependencies

### Database
- **PostgreSQL**: Primary database, configured via DATABASE_URL or individual PGUSER/PGPASSWORD/PGHOST/PGPORT/PGDATABASE environment variables
- **psycopg2-binary**: PostgreSQL adapter for Python

### Notification Services
- **Telegram Bot API**: For real-time notifications (requires bot token configuration)
- **SMTP Email**: Configured via `smtp_config.json` for email notifications

### Environment Variables Required
- `SESSION_SECRET`: Flask secret key for session encryption
- `DATABASE_URL`: PostgreSQL connection string (or individual PG* variables)
- `ADMIN_USERNAME`: Platform administrator username
- `ADMIN_PASSWORD`: Platform administrator password

### Frontend CDN Dependencies
- Bootstrap 5.1.3/5.3.0
- Font Awesome 6.x
- Google Fonts (Poppins)

### Deployment Platforms
- **Replit**: Primary development environment with PostgreSQL provisioning
- **Render**: Production deployment using `render.yaml` and `Procfile` with Gunicorn