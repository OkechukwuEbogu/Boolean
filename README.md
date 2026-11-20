# Trip Planner

A React Native (Expo) mobile app for discovering places to visit and turning them into a day-by-day itinerary. Search for a destination, browse attractions, add places to a trip, schedule each one by day and time, and see the plan with a weather forecast for each day.

> Final Term Project, Phase 1 and beyond. Built with live third-party APIs, with no database.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Features](#features)
3. [Screens and Navigation](#screens-and-navigation)
4. [APIs Used](#apis-used)
5. [Tech Stack](#tech-stack)
6. [Setup Steps](#setup-steps)
7. [Project Structure](#project-structure)
8. [How the Course Requirements Are Met](#how-the-course-requirements-are-met)
9. [Screenshots and Screen Flow](#screenshots-and-screen-flow)
10. [Team Contributions](#team-contributions)
11. [Development Timeline](#development-timeline)
12. [Known Limitations](#known-limitations)
13. [Future Work](#future-work)

---

## Project Overview

Travelers usually use one app to search for attractions, another to check the weather, and a notes app or spreadsheet to build the plan. Trip Planner brings those steps into one place.

**Target users:** travelers and vacation planners, students planning weekend trips, families and groups, and locals exploring their own region (for example, Albertans planning a trip to Banff or Jasper).

All place and weather data is fetched live from public APIs using Axios and async/await. Trips are held in app state while the app is open.

## Features

**Core**

- Destination search with autosuggest
- Attractions list with category filters (Nature, Landmarks, Museums, and more)
- Place detail with image, address, and description
- Create a trip with a name, destination, start date, and end date
- Add a place to a trip and choose the day, start time, and duration
- Itinerary view with a tab for each day and stops in time order
- Daily weather forecast for each trip day
- Loading, empty, and error states with retry

**Planned enhancements (if time allows)**

- Rain suggestion: flag outdoor stops on rainy days and suggest indoor alternatives
- Schedule warnings for overlapping times or an overly busy day
- Per-place budget with a running trip total
- Notes on each place
- Move-up and move-down buttons to reorder stops

## Screens and Navigation

| Screen | Purpose | Status |
| --- | --- | --- |
| Home / Explore | Search a city or pick a quick-start destination (Banff, Jasper, Calgary) | Prototype built |
| Places List | Attractions for a destination, with weather chip and category chips | Prototype built |
| Place Detail | Full information about one place and an "Add to trip" button | Prototype built |
| My Trips | List of trips and a "Plan a new trip" button | Prototype built |
| Trip Setup | Form to create a trip with dates | Planned |
| Add to Trip (bottom sheet) | Choose trip, day, start time, and duration | Planned |
| Itinerary | Day tabs, daily weather, and the time-ordered stops | Planned |

*Update the Status column as screens are completed.*

**Navigation:** a bottom tab navigator with two tabs, **Explore** and **My Trips**. Each tab has its own stack navigator.

```
Explore tab:   Home  ->  Places List  ->  Place Detail  ->  Add to Trip (sheet)
My Trips tab:  My Trips  ->  Trip Setup
               My Trips  ->  Itinerary
```

## APIs Used

| API | Used for | Key required |
| --- | --- | --- |
| [OpenTripMap](https://opentripmap.io) | Destination lookup, nearby places of interest, autosuggest, and place details | Yes (free) |
| [Open-Meteo](https://open-meteo.com) | Current conditions and daily forecast for a destination | No |

Notes:

- The OpenTripMap free tier has a daily request limit. The app debounces search input and reuses results already loaded in state to stay within it.
- Open-Meteo forecasts cover roughly the next two weeks, so trip days beyond that show "forecast not available yet."
- Quick-start Alberta destinations (Banff, Jasper, Calgary) show a short curated list of places. Cities found through search load their places from OpenTripMap.

## Tech Stack

- [React Native](https://reactnative.dev) with [Expo](https://expo.dev)
- [React Navigation](https://reactnavigation.org) (bottom tabs and native stack)
- [Axios](https://axios-http.com) for API requests
- React Hooks (`useState`, `useEffect`, `useReducer`, `useContext`) and custom hooks
- `@expo/vector-icons` for icons
- `@react-native-community/datetimepicker` for date and time selection

## Setup Steps

### Prerequisites

- [Node.js](https://nodejs.org) (LTS version)
- npm (included with Node.js)
- The **Expo Go** app on your phone (iOS or Android), or an Android emulator / iOS simulator
- A free OpenTripMap API key (see below)

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/TripPlanner.git
cd TripPlanner
```

### 2. Install dependencies

```bash
npm install
```

If you are setting the project up from scratch, install the main packages with:

```bash
npm install axios @react-navigation/native @react-navigation/bottom-tabs @react-navigation/native-stack
npx expo install react-native-screens react-native-safe-area-context @react-native-community/datetimepicker
```

### 3. Get an OpenTripMap API key

1. Register for a free key on the [OpenTripMap website](https://opentripmap.io).
2. Copy your key.

### 4. Add your key to the environment file

Copy the example file and paste in your key:

```bash
cp .env.example .env
```

Then edit `.env`:

```
EXPO_PUBLIC_OPENTRIPMAP_KEY=your_key_here
```

> `.env` is listed in `.gitignore` and must **not** be committed. Variables that start with `EXPO_PUBLIC_` are included in the app bundle, which is fine for a class project but not for a production secret.

### 5. Start the app

```bash
npx expo start
```

Scan the QR code with Expo Go (Android) or the Camera app (iOS), or press `a` for an Android emulator or `i` for an iOS simulator.

### Troubleshooting

- **No places load:** check that the key in `.env` is correct, then restart with `npx expo start -c` to clear the cache.
- **Request limit errors:** wait and try again later, since the free tier has a daily limit.
- **Phone cannot connect:** make sure the phone and computer are on the same Wi-Fi network.

## Project Structure

```
TripPlanner/
  App.js
  .env                (not committed)
  .env.example
  src/
    api/              (tripMapApi.js, weatherApi.js)
    components/       (PlaceCard, TripCard, DayTab, ItineraryItem, SearchBar, ...)
    context/          (TripContext.js, tripReducer.js)
    hooks/            (useApiData.js, usePlaces.js, useWeather.js, useTrips.js)
    navigation/       (TabNavigator.js, ExploreStack.js, TripsStack.js)
    screens/          (HomeScreen.js, PlacesScreen.js, PlaceDetailScreen.js,
                       TripsScreen.js, TripSetupScreen.js, ItineraryScreen.js)
    utils/            (dateHelpers.js, scheduleChecks.js, formatters.js)
    theme/            (colors.js, spacing.js, typography.js)
  assets/
  docs/               (proposal, wireframes)
  README.md
```

## How the Course Requirements Are Met

| Requirement | How it is used |
| --- | --- |
| At least 3 screens | Home, Places List, Place Detail, My Trips, Trip Setup, Itinerary |
| Box model and Flexbox | `StyleSheet` layouts using `flex`, `flexDirection`, `justifyContent`, `alignItems`, `padding`, `margin`, and `position` |
| React Hooks | `useState` for forms and filters, `useEffect` for data fetching, `useReducer` with Context for trips, and custom hooks |
| API integration with Axios | One Axios instance per API, with async/await and try/catch error handling |
| React Navigation | Bottom tabs plus a stack navigator in each tab |
| FlatList | Destination cards, attractions, trips, and the stops in each itinerary day |
| Icons and images | Expo Vector Icons, place images from the API, and local placeholders |
| Reusable components | `Header`, `SearchBar`, `PlaceCard`, `CategoryChip`, `TripCard`, `DayTab`, `ItineraryItem`, `WeatherChip`, `LoadingView`, `ErrorMessage`, `EmptyState` |
| Custom hook | `useApiData(client, url, params)` returns data, loading, error, and refetch |

## Screenshots and Screen Flow

### Wireframes

The full wireframe set is in [`docs/wireframes/TripPlanner_Wireframes.pdf`](docs/wireframes/TripPlanner_Wireframes.pdf).

### App screenshots

Add screenshots to a `screenshots/` folder and update the file names below.

| Home / Explore | Places List | Place Detail | My Trips |
| --- | --- | --- | --- |
| ![Home](screenshots/home.png) | ![Places List](screenshots/places-list.png) | ![Place Detail](screenshots/place-detail.png) | ![My Trips](screenshots/my-trips.png) |

| Trip Setup | Add to Trip | Itinerary |
| --- | --- | --- |
| ![Trip Setup](screenshots/trip-setup.png) | ![Add to Trip](screenshots/add-to-trip.png) | ![Itinerary](screenshots/itinerary.png) |

## Team Contributions

| Team member | Responsibilities |
| --- | --- |
| [Student 1 name] | Project setup, navigation, theme, Home screen, OpenTripMap integration |
| [Student 2 name] | Attractions list and place detail screens, search and filters, `useApiData` hook |
| [Student 3 name] | Trip context and reducer, trip setup and itinerary screens, weather integration, README, testing |

*Replace the names and adjust the responsibilities to match what each person actually did. Every member should commit to the repository.*

## Development Timeline

The project is due on **December 14, 2026**.

| Week | Dates | Focus |
| --- | --- | --- |
| 1 | Nov 17 to Nov 23 | Proposal, API key, GitHub repo, wireframes, Expo setup, navigation, theme, base components, `useApiData` hook |
| 2 | Nov 24 to Nov 30 | Destination search, attractions list, filters, place detail, trip state, My Trips and Trip Setup screens |
| 3 | Dec 1 to Dec 7 | Add to Trip form, Itinerary screen with edit and remove, daily weather, feature freeze on Dec 7 |
| 4 | Dec 8 to Dec 14 | Error handling, loading states, layout fixes, device testing, screenshots, final README, submission |

## Known Limitations

- **Trips are held in app state only.** There is no database or local storage, so trips are cleared when the app reloads or closes.
- **Forecast range.** Weather is available only for roughly the next two weeks.
- **Place data varies.** Some places from OpenTripMap have no image or a short description, so the app uses placeholders and fallback text.
- **API limits.** The free OpenTripMap tier has a daily request limit.

## Future Work

- Share itineraries with friends or publish them publicly (requires accounts and a backend)
- Save trips on the device between sessions
- Maps and routing between stops
- Booking links for hotels and activities
- Drag and drop reordering of stops

## Acknowledgements

- Place data from [OpenTripMap](https://opentripmap.io)
- Weather data from [Open-Meteo](https://open-meteo.com)
- Photos and descriptions for curated destinations: [add sources here]
