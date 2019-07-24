# iOS-Persistence-and-Core-Data
iOS Persistence and Core Data - Udacity Free Course

#### What is Persistence
Persistence is saving data to a place where it can be re-accessed and retrieved upon restart of the device or app.

### UserDefault
* UserDefault: String, Number, Date, Array, Dictionary.
* UserDefault: Should be less than 1MB.
* UserDefault: is for small user preferences.
```swift
func checkIfFirstLaunch() {
    if UserDefaults.standard.bool(forKey: "HasLaunchedBefore") {
        print("App has launched before")
    } else {
        print("This is the first launch ever!")
        UserDefaults.standard.set(true, forKey: "HasLaunchedBefore")
        UserDefaults.standard.set(0.0, forKey: "Slider Value Key")
        UserDefaults.standard.synchronize()
    }
}

func application(_ application: UIApplication, willFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    checkIfFirstLaunch()
    return true
}
```
