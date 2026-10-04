# ReadyDonor — Client

ReadyDonor is a blood donation platform frontend that helps people find blood donors, browse donation requests, and manage donation-related activities. It includes public pages, Firebase-based authentication, and a role-aware dashboard for donors, volunteers, and administrators.

This repository contains the **client-side application**. It communicates with the ReadyDonor backend through an API and uses Firebase Authentication for sign-in.

## Features

### Public pages
- **Home:** Landing page with donation-related content, latest requests/donors sections, and a contact section.
- **Find Donor:** Filter and search for registered blood donors.
- **Blood Donations:** Browse pending blood donation requests with pagination.
- **Donation Details:** View a specific donation request and access its donation flow.
- **Funding:** Authenticated users can access the funding page.
- **Authentication:** Register and log in using Firebase Authentication.

### Dashboard
The dashboard is protected by authentication and includes role-based navigation and page access.

- **My Dashboard:** User dashboard with role-specific content.
- **Profile:** View and manage profile information.
- **My Donation Requests:** View a user's donation requests.
- **Create Donation Request:** Submit a new blood donation request.
- **Edit Donation Request:** Update an existing request.
- **All Users:** Admin-only user management.
- **All Blood Donation Requests:** Available to admins and volunteers for managing requests.

### User experience
- Responsive layouts styled with Tailwind CSS and daisyUI.
- Loading indicators for asynchronous operations.
- Toast notifications and confirmation dialogs.
- Theme toggle control.
- Route-level error/not-found page.

## Tech Stack

| Technology | Purpose |
|---|---|
| React 19 | Component-based user interface |
| Vite | Development server and build tool |
| React Router 7 | Client-side routing and nested layouts |
| Tailwind CSS 4 | Utility-first styling |
| daisyUI 5 | UI components and themes |
| Firebase Authentication | User authentication |
| Axios | Backend API communication |
| React Hook Form | Form handling |
| React Icons | Icons |
| React Toastify | Toast notifications |
| SweetAlert2 | Confirmation and alert dialogs |
| Swiper | Carousel/slider UI |
| ESLint | Code linting |

## Project Structure

```text
src/
├── assets/
│   └── images/                 # Local image assets
├── components/                 # Shared UI components
├── contexts/                   # Authentication and database-user contexts
├── firebase/                   # Firebase initialization
├── hooks/                      # Custom hooks (auth, Axios, user data, etc.)
├── layouts/                    # Main, auth, and dashboard layouts
├── pages/
│   ├── auth/                   # Login and registration
│   ├── bloodDonations/         # Public donation listing
│   ├── dashboard/              # Dashboard pages and subcomponents
│   ├── error/                  # Not-found/error page
│   ├── findDonor/              # Donor search and filters
│   ├── funding/                # Funding page
│   ├── home/                   # Landing page and sections
│   └── loadingPage/            # Full-page loading UI
├── router/                     # Route definitions and route guards
├── index.css                   # Global styles
└── main.jsx                    # Application entry point
```

## Getting Started

### Prerequisites
- Node.js (use a current LTS release)
- npm
- Access to a running ReadyDonor backend
- A Firebase project with Authentication configured

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd ready-donor-client
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Create a `.env` file in the project root:

```env
VITE_SERVER_LINK=http://localhost:5000

VITE_apiKey=your_firebase_api_key
VITE_authDomain=your_firebase_auth_domain
VITE_projectId=your_firebase_project_id
VITE_storageBucket=your_firebase_storage_bucket
VITE_messagingSenderId=your_firebase_messaging_sender_id
VITE_appId=your_firebase_app_id
```

Set `VITE_SERVER_LINK` to the base URL of your backend. The client appends `/api` when creating its Axios instances, so the resulting API base URL is:

```text
<VITE_SERVER_LINK>/api
```

Replace the Firebase values with the configuration from your own Firebase project. The Firebase web configuration is used by the client; do not put server secrets, private keys, or service-account credentials in Vite environment variables.

> Vite exposes variables prefixed with `VITE_` to client-side code. Treat these values as public and never store confidential credentials in them.

### 4. Start the development server

```bash
npm run dev
```

Vite will print the local development URL in the terminal.

## Available Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start the Vite development server |
| `npm run build` | Create a production build in `dist/` |
| `npm run preview` | Preview the production build locally |
| `npm run lint` | Run ESLint |

## Routing Overview

| Route | Access | Description |
|---|---|---|
| `/` | Public | Redirects to `/home` |
| `/home` | Public | Home page |
| `/find-donor` | Public | Search for donors |
| `/blood-donations` | Public | Browse pending donation requests |
| `/blood-donations/details/:id` | Public route | Donation request details |
| `/funding` | Authenticated | Funding page |
| `/auth/login` | Guest-facing | Login |
| `/auth/register` | Guest-facing | Registration |
| `/dashboard` | Authenticated | Redirects to the user's dashboard |
| `/dashboard/my-dashboard` | Authenticated | Dashboard overview |
| `/dashboard/profile` | Authenticated | Profile |
| `/dashboard/my-donation-requests` | Donor/Admin | Manage own donation requests |
| `/dashboard/my-donation-requests/edit/:id` | Donor/Admin | Edit a request |
| `/dashboard/create-donation-request` | Donor/Admin | Create a request |
| `/dashboard/all-users` | Admin | User management |
| `/dashboard/all-blood-donation-requests` | Admin/Volunteer | Manage all donation requests |

Route guards in the client improve navigation and user experience. **The backend must independently verify authentication, ownership, and permissions for every protected operation.** Client-side role checks are not a security boundary.

## Authentication and API

- Firebase Authentication manages the signed-in user.
- The application has auth and database-user contexts to make authentication state and user profile/role data available across the UI.
- `useAxios` provides an Axios instance for regular API requests.
- `useAxiosSecure` attaches the current Firebase ID token as a Bearer token for authenticated requests and handles common authorization errors.
- The backend is expected to validate the token and enforce authorization rules.

Ensure the backend's allowed CORS origins include the frontend's local and deployed URLs.

## Deployment Notes

1. Configure the production `VITE_SERVER_LINK` and Firebase environment values in your hosting provider.
2. Run `npm run build`.
3. Deploy the generated `dist/` directory to a static hosting provider.
4. Configure the host to serve `index.html` for client-side routes. The repository includes a `public/_redirects` file for hosts that support that redirect format; configure equivalent rewrites if your host uses another system.
5. Configure Firebase Authentication's authorized domains and the backend's CORS settings for the deployed domain.

## Notes

- This README documents the client application. Backend setup, database configuration, API endpoint details, and server deployment belong in the ReadyDonor server repository's documentation.
- Keep `.env` files out of version control.
- The app's dashboard uses roles such as `admin`, `volunteer`, and `donor`; the backend should remain the source of truth for permissions.

## License

No license is specified in this repository. Add a license file and update this section if you intend to distribute the project under a particular license.
