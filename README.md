# WorldWise 🌍

A modern React travel tracking application that helps you keep track of your adventures around the world. Mark cities you've visited on an interactive world map, add notes about your experiences, and view your travel history in an organized way.

![WorldWise](https://img.shields.io/badge/React-18.2.0-blue) ![Vite](https://img.shields.io/badge/Vite-4.4.5-purple) ![License](https://img.shields.io/badge/license-MIT-green)

## ✨ Features

- 🗺️ **Interactive World Map** - Click anywhere on the map to add cities you've visited
- 🏙️ **City Tracking** - Save and view all the cities you've explored
- 📝 **Travel Notes** - Add personal notes and memories about each destination
- 📅 **Date Tracking** - Record when you visited each city
- 🌎 **Country View** - See your travels organized by country
- 🔐 **Authentication** - Secure login system to protect your travel data
- 📱 **Responsive Design** - Beautiful UI that works on all devices

## 🛠️ Tech Stack

- **React 18.2.0** - UI library
- **Vite 4.4.5** - Build tool and dev server
- **React Router DOM 6.30.1** - Client-side routing
- **React Leaflet 4.2.1** - Interactive map components
- **Leaflet 1.9.4** - Open-source mapping library
- **React DatePicker 8.8.0** - Date selection component
- **JSON Server 1.0.0** - Mock REST API for development
- **CSS Modules** - Scoped styling

## 🚀 Getting Started

### Prerequisites

- Node.js (v16 or higher)
- npm or yarn

### Installation

1. Clone the repository:

```bash
git clone <repository-url>
cd worldwise-2
```

2. Install dependencies:

```bash
npm install
```

3. Start the development server:

```bash
npm run dev
```

4. (Optional) Start the JSON server for mock API:

```bash
npm run server
```

The application will be available at `http://localhost:5173` (or the port Vite assigns).

## 📜 Available Scripts

- `npm run dev` - Start the development server
- `npm run build` - Build the app for production
- `npm run preview` - Preview the production build locally
- `npm run lint` - Run ESLint to check code quality
- `npm run server` - Start JSON server on port 8000

## 🔑 Authentication

The app currently uses a fake authentication system for development purposes.

### Login Credentials:

- **Email:** `sajesh@example.com`
- **Password:** `ggez`

### Authentication Flow:

1. Login with credentials above
2. Upon successful authentication, you'll be redirected to `/app`
3. Your session persists while using the app
4. Use the logout button to sign out

> **Note:** This is a development-only authentication system. For production, integrate with a real authentication service.

## 📁 Project Structure

```
worldwise-2/
├── src/
│   ├── components/       # Reusable UI components
│   │   ├── AppNav.jsx
│   │   ├── BackButton.jsx
│   │   ├── Button.jsx
│   │   ├── City.jsx
│   │   ├── CityItem.jsx
│   │   ├── CityList.jsx
│   │   ├── CountryList.jsx
│   │   ├── Form.jsx
│   │   ├── Logo.jsx
│   │   ├── Map.jsx
│   │   ├── Message.jsx
│   │   ├── PageNav.jsx
│   │   ├── Sidebar.jsx
│   │   ├── Spinner.jsx
│   │   ├── SpinnerFullPage.jsx
│   │   └── User.jsx
│   ├── contexts/          # React Context providers
│   │   ├── AuthContext.jsx
│   │   └── CitiesContext.jsx
│   ├── hooks/            # Custom React hooks
│   │   ├── useGeolocation.js
│   │   └── useUrlPosition.js
│   ├── pages/            # Page components
│   │   ├── AppLayout.jsx
│   │   ├── Homepage.jsx
│   │   ├── Login.jsx
│   │   ├── PageNotFound.jsx
│   │   ├── Pricing.jsx
│   │   ├── Product.jsx
│   │   └── ProtectedRoute.jsx
│   ├── App.jsx           # Main app component with routing
│   └── main.jsx          # App entry point
├── data/
│   └── cities.json       # Mock data for JSON server
├── public/                # Static assets
└── package.json
```

## 🗺️ How to Use

1. **Login** - Start by logging in with the credentials provided above
2. **Explore the Map** - Navigate to the map view to see the world
3. **Add a City** - Click anywhere on the map to add a city you've visited
4. **Fill Details** - Add the visit date and notes about your experience
5. **View Your Cities** - Check out the cities list to see all your travels
6. **See Countries** - View your travels organized by country

## 🔧 Configuration

### Environment Variables

Currently, no environment variables are required. For production, you may want to:

- Set up API endpoints
- Configure authentication providers
- Add API keys for geocoding services

### API Endpoints

The app uses JSON Server for mock data:

- Base URL: `http://localhost:8000`
- Cities endpoint: `/cities`

## 🎨 Styling

The project uses CSS Modules for component-level styling. Each component has its corresponding `.module.css` file for scoped styles.

## 📝 Development Notes

- The authentication system uses React Context API with `useReducer`
- City data is managed through `CitiesContext`
- Map interactions are handled with React Leaflet
- Navigation is implemented with React Router

## 🐛 Known Issues

- Fast Refresh warning in `AuthContext.jsx` (doesn't affect functionality)
- Authentication is fake/for development only
- No error handling for invalid login attempts

## 🚧 Future Enhancements

- [ ] Real authentication backend integration
- [ ] Protected routes implementation
- [ ] Session persistence (localStorage)
- [ ] Error handling and user feedback
- [ ] Password validation
- [ ] User registration
- [ ] Photo uploads for cities
- [ ] Share travel logs with friends

## 📄 License

This project is private and for educational purposes.

## 👤 Author

Sajesh

---

**Happy Traveling! ✈️**
