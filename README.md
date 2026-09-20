# Event Management System (Website)

A Django-based web application tailored for managing and tracking event registrations. The system provides a structured backend to handle both individual and team-based participations, with integrated checks for event capacities, registration deadlines, and payment verifications.

## Core Domain Models & Features

- **Events (`Events`)**: Represents an occurrence requiring registration. Supports parameters such as capacity limits, hard deadlines, paid vs. free configurations, and event types (Individual vs. Team).
- **Registrations (`Registration`)**: Tracks the primary registrant. Features built-in Django validation rules (`clean()`) to ensure registrations are blocked if the event has surpassed capacity or missed the deadline. For paid events, it mandates uploading a payment screenshot.
- **Team Management (`TeamMember`)**: Allows appending multiple sub-members to a primary `Registration` object. Validators enforce that team members cannot be added to events strictly flagged as 'individual'.
- **Automated Capacity Tracking**: Uses Django Signals (`post_save`) to automatically atomically increment the `current_registration` counter via SQL `F()` expressions upon successful registration, avoiding race conditions.

## Prerequisites

- **Python 3.8+**
- **Django 4.0+**
- **Pillow** (Required for `ImageField` handling, e.g., posters and payment screenshots)

## Installation & Setup

1. **Clone the repository**:
   ```bash
   git clone <repo-url>
   cd website
   ```
2. **Setup virtual environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate
   ```
3. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
4. **Run Migrations**:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

## Usage / Running Locally

To start the local development server:
```bash
python manage.py runserver
```

You can manage the models via the Django admin panel (if a superuser is created). Media uploads (like event posters and payment screenshots) will be saved in the `media/` directory.

## Project Structure

```text
.
├── website/             # Django root configuration (settings, urls)
├── users/               # Application logic focusing on Event and Registration schemas
│   ├── models.py        # Core domain models (Events, Registration, TeamMember)
│   ├── views.py         # HTTP Handlers
│   └── serializers.py   # Data validation and serialization
├── members/             # Auxiliary member application logic
├── media/               # Uploaded content (EventImage, image_poster, payment_screenshots)
└── manage.py            # CLI entry point
```
