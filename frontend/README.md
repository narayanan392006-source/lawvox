# LAWVOX Frontend

Next.js-based frontend for LAWVOX - Constitutional Case Research Platform

## Features

- ⚖️ Browse Constitutional Cases
- 🔖 Bookmark Important Cases
- 🎧 Audio Case Playback with Progress Tracking
- 📝 Research Notes for Case Analysis
- 🔍 Full-Text Search
- 📊 Personalized Dashboard
- 👤 User Profile Management
- ⚙️ Application Settings

## Tech Stack

- **Framework**: Next.js 15
- **UI**: React 19 + Radix UI
- **Styling**: Tailwind CSS
- **Language**: TypeScript
- **HTTP Client**: Fetch API

## Installation

```bash
cd frontend
npm install
```

## Development

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## Build

```bash
npm run build
npm start
```

## Project Structure

```
frontend/
├── src/
│   ├── app/              # Next.js app router pages
│   ├── components/       # React components
│   ├── services/         # API client functions
│   ├── context/          # React Context
│   ├── types/            # TypeScript types
│   └── data/             # Static data
├── public/               # Static assets
└── package.json          # Dependencies
```

## Pages

- `/` - Dashboard (home)
- `/library` - Browse all cases
- `/search` - Search results
- `/bookmarks` - Saved cases
- `/history` - Listening history
- `/notes` - Research notes
- `/profile` - User profile
- `/settings` - Application settings

## Environment Variables

Create `.env.local` file:

```
NEXT_PUBLIC_API_BASE_URL=http://localhost:5000
```

## API Integration

All API calls go to `http://localhost:5000/api/`

- `GET /api/cases` - List all cases
- `GET /api/cases/:id` - Get case details
- `GET /api/search?q=keyword` - Search cases
- `GET /api/bookmarks` - Get bookmarks
- `POST /api/bookmarks/:id` - Add bookmark
- `GET /api/history` - Get listening history
- `POST /api/notes` - Create note
- `GET /api/profile` - Get user profile
- `GET /api/settings` - Get app settings
