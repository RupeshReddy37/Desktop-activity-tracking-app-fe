# tracking-frontend

React + Vite frontend for the `tracking-server` backend. Extracted from desktop-tracking-system for independent development and deployment.

## Features

- 🚀 **Performance Optimized**: Request caching, code-splitting, lazy loading
- 📊 **Dashboard**: Real-time employee monitoring and activity tracking
- 📈 **Reports**: Daily summaries, trends, and anomaly detection
- 🎯 **Categories**: Productivity classification for applications and websites
- 🔐 **Secure**: JWT token authentication and role-based access control

## Quick Start

### Installation

```bash
npm install
```

### Development

```bash
npm run dev
```

The app runs on `http://localhost:5173` and proxies `/api` requests to `http://localhost:8081` (configurable in `vite.config.js`).

### Production Build

```bash
npm run build
npm run preview
```

Optimizations include:
- Code splitting (React vendor, UI vendor, page chunks)
- Minification with Terser
- Console log removal
- Source map disabled

## Backend Routes

This frontend integrates with the following backend API endpoints:

### Authentication
- `POST /api/v1/auth/refresh` - Refresh access token

### Dashboard
- `GET /api/v1/dashboard/home` - Get live employee dashboard
- `GET /api/v1/reports/daily-app-usage` - Get application usage by category

### Employees
- `POST /api/v1/employees` - Register new employee
- `GET /api/v1/dashboard/employees/{employeeId}/profile` - Get employee profile

### Devices
- `POST /api/v1/devices/register` - Register new device

### Reports
- `GET /api/v1/reports/daily-summary` - Get daily trend data
- `GET /api/v1/reports/anomalies` - Get system-wide anomalies
- `GET /api/v1/reports/employees/{employeeId}/anomalies` - Get employee-specific anomalies

### Configuration
- `GET /api/v1/categories` - List all categories
- `POST /api/v1/categories` - Create category
- `PUT /api/v1/categories/{id}` - Update category
- `DELETE /api/v1/categories/{id}` - Delete category
- `GET /api/v1/category-rules` - List classification rules
- `POST /api/v1/category-rules` - Create rule
- `PUT /api/v1/category-rules/{id}` - Update rule
- `DELETE /api/v1/category-rules/{id}` - Delete rule
- `POST /api/v1/category-rules/classify` - Test classification

## Environment Configuration

Edit `vite.config.js` to change the backend API URL:

```javascript
proxy: {
  '/api': {
    target: 'http://your-backend-url:8080',
    changeOrigin: true
  }
}
```

## Architecture

### Directory Structure

```
src/
├── components/          # Reusable UI components
│   ├── charts/         # Chart visualizations
│   ├── layout/         # App layout (sidebar, topbar)
│   ├── profile/        # Employee profile drawer
│   ├── routing/        # Route protection
│   └── ui/             # Primitives (buttons, fields, etc)
├── context/            # React context (Auth)
├── pages/              # Page components
├── services/           # API communication
│   ├── api.js          # HTTP client with caching
│   ├── cache.js        # Request cache with TTL
│   ├── endpoints.js    # API endpoint definitions
│   ├── auth.js         # Auth helpers
│   └── tokenStorage.js # Token persistence
├── lib/                # Utility functions
├── utils/              # Helper functions
│   ├── aggregate.js    # Data aggregation
│   ├── csv.js          # CSV export
│   ├── date.js         # Date utilities
├── App.jsx             # App routing
├── main.jsx            # Entry point
└── styles.css          # Global styles
```

## Performance Optimizations

### 1. Request Caching
Automatically caches GET requests for 5 minutes:
```javascript
import { getCached, setCached } from './services/cache';

const cached = getCached(path);
if (cached) return cached;
```

### 2. Code Splitting
Pages are lazy-loaded:
```javascript
const DashboardPage = lazy(() => import('./pages/Dashboard'));
```

### 3. Optimistic Updates
UI updates immediately while API requests complete:
```javascript
setCategories(cats => cats.map(c => c.id === id ? {...c, ...payload} : c));
await updateCategory(id, payload);
```

### 4. Efficient Data Processing
Single-pass calculations:
```javascript
const totals = trendRows.reduce((acc, row) => {
  acc.totalActive += row.active;
  acc.totalIdle += row.idle;
  return acc;
}, { totalActive: 0, totalIdle: 0 });
```

## Technologies

- **React 18** - UI framework
- **Vite 5** - Build tool
- **React Router 7** - Client-side routing
- **Lucide React** - Icon library

## Development

### Code Style
- Functional components with hooks
- Custom hooks for logic reuse
- Context API for global state
- useMemo and useCallback for optimization

### Building

The build process creates optimized bundles with:
- Separate vendor chunks for React dependencies
- Code minification
- Tree-shaking of unused code
- Browser caching via chunk hashing

## Troubleshooting

### Backend not responding
1. Verify backend is running on correct port (default: 8081)
2. Check CORS configuration on backend
3. Verify API_BASE in `vite.config.js` matches backend URL

### Session expires frequently
1. Check token expiration times on backend
2. Verify refresh token endpoint is working
3. Check browser local storage for tokens

### Slow performance
1. Check Network tab in DevTools for large requests
2. Verify cache is working (should see cached responses)
3. Check bundle size: `npm run build` and review output

## Related Projects

- [desktop-tracking-system](https://github.com/RupeshReddy37/desktop-tracking-system) - Full-stack project with backend

## License

MIT
