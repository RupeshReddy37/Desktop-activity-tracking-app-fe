# Migration Notes

This repository was extracted from `RupeshReddy37/desktop-tracking-system` on 2026-08-12.

## What Happened

The `tracking-frontend` folder has been separated into its own independent repository to:
- Enable independent frontend development and deployment
- Allow separate CI/CD pipelines
- Improve code organization and team management
- Facilitate faster iteration cycles

## Commit History

All commit history from the `tracking-frontend` folder has been preserved. The git log will show all changes that affected this frontend code.

## Next Steps

1. Update any references to this repository in your CI/CD pipelines
2. Update environment configuration if needed
3. Install dependencies: `npm install`
4. Run development server: `npm run dev`
5. Build for production: `npm run build`

## Backend Integration

To integrate with the backend (desktop-tracking-system), update the API base URL in your environment configuration. The default is `http://localhost:8080`.

See `tracking-frontend/src/services/api.js` for API configuration details.
