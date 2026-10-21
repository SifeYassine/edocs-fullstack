# eDocs (Full Stack)

**eDocs** is a full-stack **document management system**. Users upload, organize, search, and preview their documents in the browser, while administrators manage users, roles, and permissions.

This monorepo contains both halves of the app:

| Folder       | Stack | Description |
| ------------ | ----- | ----------- |
| [`backend/`](backend)   | Laravel 11, Sanctum | REST API: auth, RBAC, documents, categories, posts |
| [`frontend/`](frontend) | Vue 3, Vuex, Vue Router, Tailwind CSS, Vuesax Alpha | Single-page app |

<p align="center">
  <img src="https://raw.githubusercontent.com/SifeYassine/portfolio/main/assets/images/edocs1.png" alt="eDocs screenshot" width="48%" />
  <img src="https://raw.githubusercontent.com/SifeYassine/portfolio/main/assets/images/edocs2.png" alt="eDocs screenshot" width="48%" />
</p>

## Features

### Documents
- Upload files up to **10 MB**: `pdf`, `docx`, `xlsx`, `pptx`, `txt`, `png`, `jpg`, `jpeg`, `mp4`
- Rename, re-categorize, and delete documents
- Search by title and filter by category
- **In-browser preview**: Office files via the Microsoft Office Online viewer, PDF/TXT via an embedded viewer, and images and videos natively

### Organization
- Personal **categories** (deleting a category keeps its documents, uncategorized)
- **Posts** API (title and description, with an `is_deleted` flag)

### Access control
- **Sanctum** token authentication (register, login, logout)
- The **first registered user becomes `Admin`**; others get the `User` role
- Per-user **permissions** (`Manage Roles`, `Manage Permissions`, `Manage Users`) enforced by a custom middleware, with Admins bypassing all checks
- Admin pages to manage **users**, **roles**, and **permissions**

## Tech Stack

| Layer     | Technology |
| --------- | ---------- |
| Backend   | PHP 8.2+, Laravel 11, Laravel Sanctum, laravel-request-docs |
| Database  | MySQL (or SQLite) |
| Frontend  | Vue 3 (Vue CLI), Vue Router 4, Vuex 4, Axios |
| UI        | Tailwind CSS, Vuesax Alpha |

## Getting Started

### Prerequisites

- PHP 8.2+ and [Composer](https://getcomposer.org/)
- [Node.js](https://nodejs.org/) and npm
- MySQL (or SQLite)

```bash
git clone https://github.com/SifeYassine/edocs-fullstack.git
cd edocs-fullstack
```

### 1. Backend

```bash
cd backend
composer install
cp .env.example .env
php artisan key:generate
```

Configure the database in `backend/.env`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=edocs
DB_USERNAME=root
DB_PASSWORD=
```

Then:

```bash
php artisan migrate
php artisan storage:link
php artisan serve          # http://127.0.0.1:8000
```

API docs are available at `http://127.0.0.1:8000/request-docs`.

### 2. Frontend

In a second terminal:

```bash
cd frontend
npm install
npm run serve              # http://localhost:8080
```

The frontend expects the API at `http://127.0.0.1:8000`. If yours runs elsewhere, update the URL in `frontend/src/api/axios.js` and `frontend/src/components/ui/documents/PreviewSelectedDocument.vue`.

### 3. First login

1. Open the frontend and **register**. The first account becomes `Admin`.
2. As Admin, create permissions labeled `Manage Roles`, `Manage Permissions`, and `Manage Users`, and assign them to other users as needed.

> Office and PDF previews are rendered by external viewers, which must be able to download the file. They only work when the backend is publicly reachable (deployed or tunneled), not on `localhost`.

## API Overview

All routes are under `/api` and, except for `auth/register` and `auth/login`, require `Authorization: Bearer <token>`.

| Resource | Endpoints |
| -------- | --------- |
| Auth | `auth/register`, `auth/login`, `auth/logout` |
| Roles *(Manage Roles)* | `roles/create`, `roles/index`, `roles/update/{id}`, `roles/delete/{id}` |
| Permissions *(Manage Permissions)* | `permissions/create`, `permissions/index`, `permissions/update/{id}`, `permissions/delete/{id}`, `users/{userId}/permissions/{permissionId}/assign`, `…/revoke` |
| Users *(Manage Users)* | `users/index`, `users/update/{id}`, `users/delete/{id}` |
| Profile | `users/get_profile`, `users/edit_profile`, `users/{userId}/permissions/index` |
| Categories | `categories/create`, `categories/index`, `categories/update/{id}`, `categories/delete/{id}` |
| Documents | `documents/create`, `documents/index`, `documents/update/{id}`, `documents/delete/{id}` |
| Posts | `posts/create`, `posts/index`, `posts/delete/{id}` |

## Related Repositories

The backend and frontend are also available separately:

- [edocs](https://github.com/SifeYassine/edocs): Laravel API
- [edocs-frontend](https://github.com/SifeYassine/edocs-frontend): Vue frontend

## Author

**Sifeddine Yassine** · [@SifeYassine](https://github.com/SifeYassine)
