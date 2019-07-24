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
## Core Data
* The code you write to define your app structure and relationships is your data model.
* The data model and related code make up the data layer, and we will use core data to manage the data layer.
* Core data is useful for managing your app's data layer in general, and for persisting it on the device.
* Core Data saves (or persists) data into something called a persistent store . The store is where the data lives.
* Core Data uses two-sided relationships.
* Entities are the basic building block of a Core Data model.

### There are three different types of persistent stores Core Data supports on iOS:

    1. a SQLite store (the default)
    2. a binary store
    3. an in-memory store
##### SQlite
SQLite is almost always the right choice for your persistent store; it means your data is stored in a SQL relational database, and there are a few handy features in Core Data (like model caching during migration) that only work with the SQLite store. And since you don’t interact with the persistent store directly, you don’t need to know any SQL to use the SQLite store.
##### In-memory
The in-memory and binary stores have different characteristics in terms of memory usage and performance. The in-memory store can be appropriate when you have a small data model that can fit in memory all at once and that doesn’t need to be saved to disk – for example a cache.
##### Binary
The binary store can be appropriate when you always need the database to be read and written in its entirety—for example if you are using a file format such as CSV.

### Swift optional VS Core Data optional
The difference between Swift optionals and Core Data optional attributes is subtle. Swift optionals can be nil during runtime, and Core Data optional attributes can be nil at save time.

### Deletion Rules
For our notes relationship, choosing the Cascade rule will mean that deleting a Notebook will cause all of its referenced notes to be deleted.
For our notebook relationship in Note, choosing Nullify means that the relationship will simply be removed, but the referenced Notebook remains.

### Class Definition
When using the `Class Definition` codegen option for an entity, you can immediately start using classes that represent your entities without any further code

### Core Data Stack
* Core data provides a set of classes that work together to manage your apps data layer. 
* Together these classes are known as the core data stack. 
##### The four parts of the stack are: 
    1. one or more managed object context
    2. the managed object model
    3. the persistent store coordinator
    4. the persisting container
