# BookMySeat (Django BookMyShow Clone)

A Django-based movie ticket booking application with search, genre/language filters, seat selection, payment simulation, email confirmations, 5-minute seat reservation timeout, and an admin analytics dashboard.

## Features
- Movie listing with search, genre and language filters
- Theater showtimes per movie
- Interactive seat selection UI
- Simulated payment flow (pending → confirmed)
- Email confirmation (console backend)
- 5-minute seat reservation timeout with automatic seat release
- Admin dashboard: total revenue, most popular movies, busiest theaters
- Image uploads for movies via Django media

## Tech Stack
- Django 6
- SQLite (development)
- Bootstrap 4, Font Awesome

## Prerequisites
- Python 3.11+ recommended
- Pip

## Setup
```bash
pip install -r requirements.txt
python manage.py makemigrations
python manage.py migrate
```

Optional: create a superuser for the admin dashboard
```bash
python manage.py createsuperuser
```

## Run
```bash
python manage.py runserver
```
Open http://127.0.0.1:8000/ in your browser.

## Usage
- Register or log in
- Browse Movies, use filters for Genre/Language
- Open a movie’s theaters, click Book Now
- Select seats and proceed to payment
- Complete payment to confirm booking; check console for email confirmation
- Admins: click Dashboard link in navbar or visit /movies/admin-dashboard/

## Configuration
- Database: SQLite by default ([settings.py](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/bookmyseat/settings.py#L82-L90))
- Media: served when DEBUG=True ([settings.py](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/bookmyseat/settings.py#L57-L58))
- Email: console backend ([settings.py](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/bookmyseat/settings.py#L55))
- Allowed hosts: localhost/127.0.0.1 ([settings.py](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/bookmyseat/settings.py#L29))

To use a real email backend, configure:
```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.example.com'
EMAIL_PORT = 587
EMAIL_HOST_USER = 'you@example.com'
EMAIL_HOST_PASSWORD = 'your-app-password'
EMAIL_USE_TLS = True
```

## Payment Integration
- Current flow simulates success and triggers email confirmation
- Integration points:
  - Payment display: [payment_view](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/movies/views.py#L100-L121)
  - Payment processing: [process_payment](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/movies/views.py#L122-L135)
  - Confirmation email: [send_booking_confirmation](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/movies/utils.py#L4-L25)

## Seat Reservation Timeout
- Pending bookings auto-expire after 5 minutes, seats released:
  - [cleanup_expired_bookings](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/movies/views.py#L10-L17)
  - Called at booking start: [book_seats](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/movies/views.py#L52-L99)

## Admin Dashboard
- Route: /movies/admin-dashboard/ (superusers only)
- View: [admin_dashboard](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/movies/views.py#L137-L147)
- Template: [admin_dashboard.html](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/templates/movies/admin_dashboard.html)

## Routes
- Movies index: /movies/ ([urls.py](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/movies/urls.py#L3-L10))
- Theaters for movie: /movies/<movie_id>/theaters
- Seat booking: /movies/theater/<theater_id>/seats/book/
- Payment: /movies/payment/
- Payment process: /movies/process-payment/
- Admin dashboard: /movies/admin-dashboard/

## UI References
- Movie list and filters: [movie_list.html](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/templates/movies/movie_list.html)
- Theater list with trailer embed: [theater_list.html](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/templates/movies/theater_list.html)
- Seat selection: [seat_selection.html](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/templates/movies/seat_selection.html)
- Payment pages: [payment.html](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/templates/movies/payment.html), [payment_success.html](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/templates/movies/payment_success.html)
- Navbar: [basic.html](file:///Users/shivamrode/Desktop/THIS%20PC/djnago-bookmyshow-clone-main/templates/users/basic.html)

## Troubleshooting
- Pillow missing: install with `pip install Pillow`
- Media not loading: ensure DEBUG=True and MEDIA settings are correct
- Login required for booking: the booking flow enforces authentication
- Payment 404 after booking: ensure the payment redirect uses the named URL (fixed)
- Template errors on seat page: context keys standardized to `theater` (fixed)

## License
For internal/demo use. Add a license if you plan to distribute.

