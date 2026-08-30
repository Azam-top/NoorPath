# NoorPath Implementation Guide

## ✅ Phases Completed

### Phase 1: Backend Foundation ✓
**Database Schema & Infrastructure**
- Complete PostgreSQL schema with 7 tables
- Row-Level Security (RLS) policies for privacy
- Proper indexes and foreign key relationships
- Migration file: `supabase/migrations/001_create_base_schema.sql`

**Tables:**
- `profiles` - User profile data (location, prayer method, preferences)
- `prayer_logs` - Prayer completion tracking
- `bookmarks` - Saved Quranic verses
- `user_settings` - App preferences
- `prayer_statistics` - Prayer streaks and stats
- `mosque_locations` - Mosque directory
- `quran_cache` - API response caching

**TypeScript Types:**
- Auto-generated types in `src/types/database.ts`
- Full type safety for all database operations

### Phase 2: Authentication System ✓
**Enhanced Auth Flow**
- Password-based signup with validation
- Email/password login with profile checking
- Complete profile setup after authentication
- Proper error handling and user feedback

**Files:**
- `src/store/auth-store.ts` - Improved auth state management
- `src/app/(auth)/signup.tsx` - Password signup screen
- `src/app/(auth)/login.tsx` - Enhanced login with navigation
- `src/app/(auth)/complete-profile.tsx` - Onboarding profile setup

**Features:**
- Profile existence validation
- Automatic routing to profile setup if needed
- Prayer method selection (5 calculation methods)
- Location capture during onboarding
- Default settings creation

### Phase 3: Prayer Times System ✓
**Real Prayer Time Calculations**
- Uses Adhan.js for accurate Islamic prayer time calculations
- 5 calculation methods: ISNA, MWL, Egypt, Karachi, UAE
- Caching system (AsyncStorage) for performance
- Per-minute updates for time remaining

**Files:**
- `src/lib/prayer-times-calculator.ts` - Prayer calculations
- `src/hooks/use-prayer-times.ts` - React hooks for prayer times

**Features:**
- Get prayer times for any date/location
- Calculate time remaining until next prayer
- Determine if prayer is currently active (45-min window)
- Auto-update countdown timers
- Format times in 12h or 24h format

### Phase 4: Quran Integration ✓
**Official Quran.com API**
- Fetch complete Quran from official source
- 131+ translations available
- Advanced search capabilities
- 30-day caching for performance

**Files:**
- `src/lib/quran-api.ts` - Quran API client
- `src/hooks/use-quran.ts` - React hooks for Quran

**Features:**
- Get all Surahs with metadata
- Fetch Ayahs with translations
- Single verse lookup
- Verse search functionality
- Translation management

### Phase 5: Geolocation & Location Services ✓
**Location Management**
- Permission handling for location access
- Reverse geocoding (get city/country from coordinates)
- Location caching (24-hour TTL)
- Distance calculations (Haversine formula)

**Files:**
- `src/lib/location-service.ts` - Geolocation service

**Features:**
- Get current user location
- Use cached location (faster)
- Get location city and country
- Calculate distance between points
- Permission request UI

### Phase 6: Hijri Calendar ✓
**Islamic Calendar Support**
- Gregorian to Hijri date conversion
- Hijri to Gregorian conversion
- Islamic event dates (Eids, Hajj, Mawlid, etc.)
- Ramadan-specific utilities

**Files:**
- `src/lib/hijri-calendar.ts` - Hijri calendar utilities

**Features:**
- Convert any date to Islamic date
- Check if date is Ramadan
- Get Islamic holiday dates
- Format dates in English or Arabic
- Calculate days until Eid

### Database Hooks System ✓
**Easy Database Access**
- Comprehensive hooks for all CRUD operations
- Built-in error handling
- User isolation (RLS enforced)
- Real-time sync with Supabase

**File:** `src/hooks/use-database.ts`

**Available Hooks:**
```typescript
useProfile()              // Get/update user profile
usePrayerLogs()          // Track prayer completion
useBookmarks()           // Manage verse bookmarks
useUserSettings()        // Store app preferences
usePrayerStatistics()    // Get prayer stats
useMosques()             // Find nearby mosques
useQuranCache()          // Cache Quran API responses
```

---

## 📋 Next Phases (Ready to Build)

### Phase 7: UI Integration & Home Screen
**Update Home Screen with Real Data:**
1. Replace hardcoded prayer times with `usePrayerTimes` hook
2. Show Hijri date using `HijriCalendar.toHijri()`
3. Display next prayer countdown
4. Show current prayer status
5. Sync prayer logs with backend via `usePrayerLogs`

**Files to Update:**
- `src/app/(app)/(tabs)/index.tsx` - Home screen

**Tasks:**
- Create Prayer Time Card component
- Create Hijri Date Display component
- Update Prayer Tracker component
- Add loading states
- Add error handling

### Phase 8: Quran Reader Screen
**Build Quran Reading Experience:**
1. Create Surah list screen (browsable)
2. Create verse display screen (with RTL Arabic text)
3. Add translation selector
4. Implement verse bookmarking
5. Add search functionality

**Files to Create:**
- `src/app/(app)/quran.tsx` - Main Quran screen
- `src/components/quran/` - Quran-specific components
  - SurahList.tsx
  - VerseCard.tsx
  - VerseDetail.tsx
  - TranslationSelector.tsx

**Integration:**
- Use `useSurahs()` to load all Surahs
- Use `useAyahs()` to load verses
- Use `useBookmarks()` to save favorites
- Show translations from `useTranslations()`

### Phase 9: Mosque Finder
**Location-Based Mosque Directory:**
1. Get user location via `LocationService.getLocation()`
2. Find nearby mosques via `useMosques().getMosquesByLocation()`
3. Show on map (or list with distance)
4. Display mosque prayer times
5. Show contact information

### Phase 10: Settings Screen
**User Preferences:**
1. Prayer calculation method selection
2. Theme toggle (dark/light)
3. Notification preferences
4. Language selection
5. Location management
6. Translation preference

**Files to Update:**
- `src/app/(app)/(tabs)/more.tsx` - Settings

### Phase 11: Polish & UX
**Visual Refinements:**
1. Add loading skeleton screens
2. Implement error boundaries
3. Add smooth animations
4. Dark/light mode support
5. Accessibility improvements
6. Performance optimization

### Phase 12: Testing & Deployment
**Quality Assurance:**
1. Unit tests for core libraries
2. Integration tests for API calls
3. E2E testing of auth flow
4. Device testing (iOS/Android)
5. Performance profiling
6. Bug fixes and polish

---

## 🚀 Development Workflow

### Running the App
```bash
cd C:\Users\barot\Desktop\Uni photoes\UP\NoorPath

# Install dependencies
npm install

# Start development server
npm start

# Run on specific platform
npm run ios     # iOS simulator
npm run android # Android emulator
npm run web     # Web browser
```

### Deploying to Supabase
The migration file needs to be applied:
```bash
# Push schema to your Supabase project
supabase db push
```

Or use Supabase dashboard to run the SQL directly.

### Environment Variables
Make sure `.env` has:
```
EXPO_PUBLIC_SUPABASE_URL=https://rkfauodfuygejfrorcvt.supabase.co
EXPO_PUBLIC_SUPABASE_PUBLISHABLE_KEY=sb_publishable_u1rU6oMZ6PzHbdmN3BR2jA_7EIVjpu5
```

---

## 🔑 Key Implementation Details

### Prayer Times
```typescript
import { usePrayerTimes } from '@/hooks/use-prayer-times';

function PrayerWidget() {
  const { prayerTimes, location, loading } = usePrayerTimes();
  
  if (loading) return <Loading />;
  
  return (
    <View>
      <Text>{location?.name}</Text>
      {prayerTimes?.allPrayers.map((prayer) => (
        <PrayerCard key={prayer.name} prayer={prayer} />
      ))}
    </View>
  );
}
```

### Quran Verses
```typescript
import { useAyahs, useBookmarks } from '@/hooks/use-quran';

function SurahScreen({ surahNumber }: { surahNumber: number }) {
  const { ayahs, loading } = useAyahs(surahNumber);
  const { addBookmark, isBookmarked } = useBookmarks();
  
  return (
    <FlatList
      data={ayahs}
      renderItem={({ item }) => (
        <VerseCard
          verse={item}
          onBookmark={() => addBookmark(surahNumber, item.numberInSurah)}
        />
      )}
    />
  );
}
```

### User Location
```typescript
import { LocationService } from '@/lib/location-service';

const location = await LocationService.getLocation();
// location.latitude, location.longitude, location.name
```

### Islamic Calendar
```typescript
import { HijriCalendar } from '@/lib/hijri-calendar';

const hijri = HijriCalendar.now();
// hijri.day, hijri.month, hijri.year, hijri.monthName

const formatted = HijriCalendar.format(hijri);
// "15 Ramadan 1445 AH"

if (HijriCalendar.isRamadan(hijri)) {
  // Show Ramadan features
}
```

---

## 📦 New Dependencies Added

```json
{
  "adhan": "^5.3.0",
  "axios": "^1.6.0", 
  "date-fns": "^3.0.0",
  "expo-location": "~17.0.0",
  "hijri-js": "^2.1.0"
}
```

Install with: `npm install`

---

## 🎯 Testing Checklist

- [ ] Signup creates profile and user settings
- [ ] Login routes to complete-profile if needed
- [ ] Prayer times calculate correctly for user location
- [ ] Prayer times update every minute
- [ ] Quran API fetches verses successfully
- [ ] Bookmarks save and load from database
- [ ] Location permission requests work on iOS/Android
- [ ] Dark/light theme toggles properly
- [ ] Offline: cached prayer times work without connection
- [ ] Offline: cached Quran verses work without connection
- [ ] Prayer logs sync to backend

---

## 🐛 Known Issues & TODOs

- [ ] Mosque locations currently has only 2 example entries
- [ ] Ramadan tracker needs Sehri/Iftar countdown
- [ ] Qibla compass not implemented
- [ ] Push notifications need setup
- [ ] Community features not started
- [ ] Prayer statistics calculation needs trigger function
- [ ] Offline sync queue for prayer logs not implemented
- [ ] Image upload for user profile not implemented

---

## 📚 Resources

- **Adhan.js Docs:** https://batoulapps.github.io/Adhan_JS/
- **Quran.com API:** https://quran.com/developers
- **Expo Location:** https://docs.expo.dev/versions/latest/sdk/location/
- **Supabase Docs:** https://supabase.com/docs
- **Hijri.js:** https://github.com/xsoh/hijri-js

---

## 🎓 Architecture Overview

```
Frontend (Expo React Native)
├── Auth System
│   ├── Signup/Login screens
│   ├── Profile completion
│   └── Session management
├── Prayer Times
│   ├── Location detection
│   ├── Prayer calculations (Adhan.js)
│   └── Real-time updates
├── Quran Reader
│   ├── Surah browsing
│   ├── Verse display
│   └── Bookmarking
└── Other Features
    ├── Mosque Finder
    ├── Settings
    └── Statistics

Backend (Supabase)
├── Auth (Supabase Auth)
├── Database (PostgreSQL)
│   ├── User profiles
│   ├── Prayer logs
│   ├── Bookmarks
│   └── Settings
└── APIs
    ├── Quran.com (official API)
    ├── Adhan.js (prayer times)
    └── Geolocation (built-in)
```

---

Generated: 2026-08-26
Status: Ready for Phase 7 (UI Integration)
