# Development Session Notes

## Date: Today's Session

---

## Issue #1: White Screen When Clicking on Individual City

### Problem Description

When clicking on an individual city from the CityList, the application was showing a white screen instead of displaying the City component with the city details.

### Root Cause Analysis

The `City` component in `src/components/City.jsx` was expecting `cities` to be passed as a prop:

```jsx
function City({ cities }) {
```

However, in `src/App.jsx`, the City component was being rendered without passing any props:

```jsx
<Route path="cities/:id" element={<City />} />
```

This caused the component to fail because `cities` was `undefined`, leading to errors when trying to find the city by ID.

### Initial Fix Applied

**File Modified:** `src/components/City.jsx`

**Changes Made:**

1. **Removed prop dependency**: Changed from `function City({ cities })` to `function City()`
2. **Added context hook**: Imported and used `useCities()` hook from `CitiesContext`
3. **Fixed city lookup**: Updated city finding logic to handle string/number ID conversion:
   ```jsx
   const city = cities.find((city) => String(city.id) === String(id));
   ```
4. **Fixed navigation path**: Changed redirect path from `"/cities"` to `"/app/cities"` to match the nested route structure

**Code Changes:**

```jsx
// Before
function City({ cities }) {
  const { id } = useParams();
  const city = cities.find((city) => city.id === id);
  if (!city) return <Navigate to="/cities" />;
  // ...
}

// After
import { useCities } from "../contexts/CitiesContext";

function City() {
  const { id } = useParams();
  const { cities } = useCities();
  const city = cities.find((city) => String(city.id) === String(id));
  if (!city) return <Navigate to="/app/cities" />;
  // ...
}
```

---

## Issue #2: Enhanced City Component with Individual City Fetching

### Problem Description

After the initial fix, the implementation was improved to fetch individual city data on demand rather than searching through the entire cities array.

### Changes Made by User

#### 1. CitiesContext Enhancement

**File Modified:** `src/contexts/CitiesContext.jsx`

**New State Added:**

- `currentCity`: State to store the currently selected city
- Fixed typo: `setCurrentyCity` (should be `setCurrentCity` - but kept as is in code)

**New Function Added:**

- `getCity(id)`: Async function to fetch individual city data from API endpoint `${BASE_URL}/cities/${id}`
  - Sets loading state to true
  - Fetches city data from API
  - Updates `currentCity` state
  - Handles errors appropriately

**Context Value Updated:**
Now provides:

- `cities` - Array of all cities
- `isLoading` - Loading state
- `getCity` - Function to fetch individual city
- `currentCity` - Currently selected city object

#### 2. City Component Refactoring

**File Modified:** `src/components/City.jsx`

**Changes Made:**

1. **Updated imports:**

   - Added `useEffect` from React
   - Added `Spinner` component
   - Added `BackButton` component
   - Removed `Navigate` import (no longer needed)

2. **Changed context usage:**

   ```jsx
   // Before
   const { cities } = useCities();
   const city = cities.find((city) => String(city.id) === String(id));

   // After
   const { currentCity, getCity, isLoading } = useCities();
   ```

3. **Added useEffect for data fetching:**

   ```jsx
   useEffect(
     function () {
       getCity(id);
     },
     [id]
   );
   ```

   - Fetches city data whenever the `id` parameter changes
   - Ensures fresh data is loaded when navigating to different cities

4. **Added loading state handling:**

   ```jsx
   if (isLoading) return <Spinner />;
   ```

   - Shows spinner while city data is being fetched

5. **Updated city data destructuring:**

   ```jsx
   const { cityName, emoji, date, notes } = currentCity;
   ```

   - Now uses `currentCity` from context instead of finding from array

6. **Added BackButton:**
   - Added BackButton component at the bottom of the city details
   - Wrapped in a div with `styles.button` class

#### 3. BackButton Component

**File:** `src/components/BackButton.jsx` (already existed)

**Functionality:**

- Uses `useNavigate` hook from react-router-dom
- Renders a Button component with type "back"
- Navigates back in history when clicked (`navigate(-1)`)
- Prevents default form submission behavior

---

## Technical Improvements Summary

### Architecture Changes

1. **Context Pattern**: Properly implemented context consumption in City component
2. **Data Fetching Strategy**: Moved from client-side filtering to server-side individual fetching
3. **Loading States**: Added proper loading indicators for better UX
4. **Navigation**: Improved navigation with back button functionality

### Code Quality

- ✅ Fixed prop drilling issue
- ✅ Improved data fetching efficiency (fetch only needed city)
- ✅ Added loading states
- ✅ Enhanced user experience with back navigation
- ✅ Better error handling through context

### Files Modified

1. `src/components/City.jsx` - Main component refactoring
2. `src/contexts/CitiesContext.jsx` - Added currentCity state and getCity function

### Files Referenced

- `src/components/BackButton.jsx` - Already existed, now being used
- `src/components/Spinner.jsx` - Already existed, now being used
- `src/App.jsx` - Route configuration (no changes needed)
- `src/components/CityList.jsx` - City list component (working correctly)
- `src/components/CityItem.jsx` - City item component (working correctly)

---

## Testing Checklist

- [ ] Click on city from CityList - should display city details
- [ ] Verify loading spinner appears while fetching
- [ ] Verify back button navigates correctly
- [ ] Test with different city IDs
- [ ] Verify error handling (if API fails)

---

## Notes

- The API endpoint structure assumes: `${BASE_URL}/cities/${id}` for individual city fetching
- Current implementation uses `useEffect` with `[id]` dependency to refetch when ID changes
- The `getCity` function in CitiesContext has an unnecessary block `{}` that could be removed for cleaner code
- There's a typo in the state setter: `setCurrentyCity` should be `setCurrentCity` but it's kept consistent in the codebase

---

## Issue #3: React-Leaflet Dependency Resolution Error

### Problem Description

When attempting to install `react-leaflet`, npm threw a dependency resolution error:

- Project uses React 18.3.1 (satisfies `^18.2.0`)
- `react-leaflet@5.0.0` requires React `^19.0.0` as a peer dependency
- This created an incompatible dependency conflict

**Error Message:**

```
Could not resolve dependency:
peer react@"^19.0.0" from react-leaflet@5.0.0
node_modules/react-leaflet
  react-leaflet@"*" from the root project
```

### Solution Applied

**File Modified:** `package.json`

**Changes Made:**

1. **Added React 18 compatible version of react-leaflet**: Installed `react-leaflet@^4.2.1` instead of version 5.x

   - Version 4.x is the latest stable version compatible with React 18
   - Version 5.x requires React 19 (which would break compatibility with the rest of the project)

2. **Added leaflet dependency**: Added `leaflet@^1.9.4` as a required peer dependency for react-leaflet
   - Leaflet is the underlying mapping library that react-leaflet wraps

**Updated Dependencies:**

```json
"dependencies": {
  "json-server": "^1.0.0-beta.3",
  "leaflet": "^1.9.4",           // Added
  "react": "^18.2.0",
  "react-dom": "^18.2.0",
  "react-leaflet": "^4.2.1",     // Added (v4 instead of v5)
  "react-router-dom": "^6.30.1"
}
```

**Installation:**

- Successfully ran `npm install`
- All packages resolved correctly without conflicts
- Installation completed with 3 new packages added

### Key Points

- **Version Compatibility**: Always check peer dependency requirements before installing packages
- **React 18 Compatibility**: Use react-leaflet v4.x for React 18 projects
- **React 19 Compatibility**: Use react-leaflet v5.x only when project is upgraded to React 19
- **Leaflet CSS**: When implementing the map component, remember to import Leaflet CSS:
  ```jsx
  import "leaflet/dist/leaflet.css";
  ```

---

## Next Steps (If Any)

- Consider adding error boundaries for better error handling
- Add error state handling in City component if API fetch fails
- Consider caching city data to avoid refetching on re-visit
- Fix typo `setCurrentyCity` to `setCurrentCity` if desired
- When implementing Map component with react-leaflet, import Leaflet CSS and set up proper map initialization
