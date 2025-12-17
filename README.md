# MarketT-Self

Live Demo: https://markett-self.vercel.app/  
Repository: https://github.com/Zhansulu300608/markett

MarketT-Self is a frontend-based marketplace web application built with Nuxt 3 and Tailwind CSS.  
The project represents a structured online market interface with user-facing pages and an admin dashboard section.

This application is designed as a portfolio and educational project demonstrating modern frontend architecture, component-based development, and responsive UI design.

---

## Project Overview

MarketT-Self simulates an online marketplace where users can browse products, view catalogs, manage favorites, and access profile and order pages.  
In addition, the project includes an admin section intended for managing products and categories.

The project focuses exclusively on frontend functionality and layout. Backend logic, authentication, and payment systems are not implemented.

---

## Application Pages

### Public Pages

- **Home (`/`)**  
  Landing page with general marketplace content.

- **Catalog (`/catalog`)**  
  Displays a list of products available in the marketplace.

- **Product Order (`/order`)**  
  Represents a product ordering interface.

- **Favourites (`/favourites`)**  
  Displays user-selected favorite products.

- **Profile (`/profile`)**  
  User profile page intended for personal data and settings.

- **Contacts (`/contacts`)**  
  Displays contact and support information.

---

### Admin Pages

Admin pages are located inside the `pages/admin` directory.

- **Admin Dashboard (`/admin/dashboard`)**  
  Main admin dashboard layout.

- **Products Management (`/admin/products`)**  
  Interface for managing product data.

- **Categories Management (`/admin/category`)**  
  Interface for managing product categories.

The admin section is UI-only and does not include authentication or data persistence.

---

## Core Features

- Modular component-based architecture  
- Separate public and admin sections  
- Product cards and catalog layout  
- Sidebar navigation for admin dashboard  
- Responsive layout for desktop and mobile devices  
- Clean and minimal UI design using Tailwind CSS  

---

## Project Structure

The project follows a clear and scalable Nuxt 3 structure:

components/
├── AppHeader.vue
├── Sidebar.vue
├── Carousel.vue
├── ProductCard.vue

pages/
├── index.vue
├── catalog.vue
├── dashboard.vue
├── favourites.vue
├── order.vue
├── profile.vue
├── contacts.vue
└── admin/
├── dashboard.vue
├── products.vue
├── category.vue

public/
├── images/
│ ├── icons8-phone-50.png
│ ├── icons8-whatsapp-logo-50.png
│ ├── icons8-email-50.png
│ └── icons8-tiktok-50.png

json/
└── banner.json

This structure ensures separation of concerns, reusability, and maintainable code organization.

---

## Technologies Used

- Framework: Nuxt 3 (Vue 3)
- Styling: Tailwind CSS
- Language: JavaScript / Vue Single File Components
- Package Manager: npm
- Deployment: Vercel
