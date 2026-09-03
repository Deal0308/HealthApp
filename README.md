# HealthApp

HealthApp is a simple calorie balance app for iOS. It reads today's active energy burned from HealthKit, lets the user manually log food entries, and shows consumed calories, burned calories, and net balance.

## Folder Breakdown

- `HealthApp/Models`
  - SwiftData models. `FoodEntry` stores the food name, calories, timestamp, and stable ID.
- `HealthApp/Services`
  - App services. `HealthKitService` owns HealthKit authorization and active-energy queries. `FoodEntryPersistenceService` owns SwiftData fetch, save, and delete operations.
- `HealthApp/ViewModels`
  - MVVM state and app logic. `CalorieBalanceViewModel` validates food input, refreshes HealthKit data, loads today's entries, and computes consumed, burned, and net calories.
- `HealthApp/Views`
  - SwiftUI views only. Views bind to ViewModel state and call ViewModel methods.
- `HealthApp/HealthAppApp.swift`
  - App entry point. Creates the SwiftData container, injects the ViewModel, and refreshes when the app becomes active.

## Persistence

Food entries are persisted with SwiftData using the `FoodEntry` model. The default SwiftData store is managed by iOS inside the app container, typically under the app's Library/Application Support storage. The app fetches only entries whose timestamp falls between local midnight and the next local midnight.

## HealthKit Setup

The project already includes the HealthKit entitlement in `HealthApp/HealthApp.entitlements`.

Before running HealthKit authorization, confirm the target has this generated Info.plist value:

- Key: `Privacy - Health Share Usage Description`
- Raw key: `NSHealthShareUsageDescription`
- Value: `HealthApp reads your active energy burned to calculate today's calorie balance.`

In Xcode, open the `HealthApp` target, go to `Info`, add a custom iOS target property, and enter the key and value above. Also confirm `Signing & Capabilities` includes `HealthKit`.

HealthKit data is best tested on a real device. On a simulator, HealthKit may have no active energy samples; the app handles this by showing 0 calories burned and an empty-data message.

## Demo Checklist

Record a 15-30 second demo showing:

1. Launch the app and accept the HealthKit permission prompt.
2. Confirm the Consumed, Burned, and Net values are visible.
3. Tap Add Food, enter a food name and calories, then save.
4. Confirm the entry appears in Today's Food and Consumed/Net update.
5. Relaunch the app and confirm the food entry is still persisted.
6. Tap Refresh to re-run the HealthKit burned-calorie query.
