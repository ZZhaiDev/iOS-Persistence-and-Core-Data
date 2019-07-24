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

### Sanbox
Each app when its installed, is placed in whats called a `sandbox`, or self—contained area of the file system in which to keep all of its stuff.
* Sanbox contains `bundle container`(application itself）, `Data container`(hold all the user and app data), `icloud container`.
* Data container contains `document`(user data, like userdefault), `library`(non-user data, like cache), `temp`(storing very temporary data that doesnot need to be persisted across launches)

#### File Manager
```swift
func sandboxPlayground() {
    let fm = FileManager.default
    let urls = fm.urls(for: .documentDirectory, in: .userDomainMask)
    let url = urls.last?.appendingPathComponent("file.txt")

    do {
        try "Hi There!".write(to: url!, atomically: true, encoding: String.Encoding.utf8)
    } catch {
        print("Error while writing")
    }

    do {
        let content = try String(contentsOf: url!, encoding: String.Encoding.utf8)
        if content == "Hi There!" {
            print("yay")
        } else {
            print("oops")
        }
    } catch {
        print("Something went wrong")
    }
}

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    sandboxPlayground()
    return true
}
```
