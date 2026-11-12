# Weather App (Kotlin, XML layouts, no Jetpack Compose)

Current conditions + 7-day forecast for your location and saved favourite cities.

## Setup (one-time)

1. Open this folder in **Android Studio** (Koala or newer). Let Gradle sync.
2. In the [Firebase console](https://console.firebase.google.com): create a project, add an Android app
   with package name `com.example.weatherapp`, and download **google-services.json**.
3. Put `google-services.json` in the `app/` folder (next to `app/build.gradle.kts`).
4. In Firebase console -> Authentication -> Sign-in method, enable **Email/Password**.
5. Run on a device/emulator with Google Play services (set a location in the emulator's extended controls).

## How requirements map to code

| Requirement | Where |
|---|---|
| Firebase Authentication | `LoginActivity.kt` (sign in / create account), `MainActivity.kt` (auth gate + sign out) |
| Fused Location Provider | `MainActivity.fetchDeviceLocation()` uses `getCurrentLocation(Priority.PRIORITY_BALANCED_POWER_ACCURACY, null)` |
| Open-Meteo request | `WeatherApi.kt` (exact URL from the brief), parsing in `Models.kt` |
| Temp, wind, icon from `weather_code` | `MainActivity.render()`, `WeatherCodes.kt`, vector icons in `res/drawable/ic_*.xml` |
| 7-day horizontal RecyclerView | `activity_main.xml` (`forecastList`, horizontal LinearLayoutManager), `DailyAdapter.kt`, `item_daily.xml` |
| Add / remove favourites | Button under the current weather + bin icon in the list (`FavouritesAdapter.kt`); stored per user in SharedPreferences |
| Pull-to-refresh | `SwipeRefreshLayout` in `activity_main.xml`; `MainActivity.refresh()` |
| Cached data when offline | `Prefs.putCache/getCache` + yellow offline banner showing when the data was fetched |
| °C/°F setting in SharedPreferences | Toolbar overflow menu -> "Use °F"; `Prefs.isFahrenheit/setFahrenheit` |

## Notes

- Open-Meteo always returns °C and km/h; the app converts to °F/mph for display, so toggling the unit
  re-renders instantly with no network call.
- If a fresh location fix isn't available, the app falls back to the last known location.
- Tap a favourite to view its weather; use the overflow menu -> "Use my location" to return to the device location.
- Gradle wrapper files are not included; Android Studio creates them on first sync.
