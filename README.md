# DataManager
DataManager is a pure swift library to manage app life-cycle data


## Overview
Almost every application, regardless of the platform, requires some form of data management. On iOS, there are three primary storage options for saving simple data without using a full-fledged database: `UserDefaults`, `Keychain`, and `FileManager`. Each of these serves a different purpose and requires separate implementations for basic operations like create, read, update, and delete (CRUD).

As a result, many apps end up using all three storage methods for different use cases, leading to fragmented code, duplicated logic, and confusion about where specific data is stored.

This library solves that problem by providing a unified interface to manage data across `UserDefaults`, `Keychain`, and `FileManager`. With a single, consistent API, you can choose the desired storage option through a simple configuration—eliminating the need for redundant code and making your data management cleaner and more maintainable.

## Installation
### Swift Package Manager (SPM)
The  [Swift Package Manager](https://swift.org/package-manager/)  is a tool for automating the distribution of Swift code and is integrated into the  `swift`  compiler.

if using in Application

-   File > Swift Packages > Add Package Dependency
-   Add  `https://github.com/sahibhussain/data-manager.git`
-   Select "Up to Next Major" with "1.1.0"

if Using in Package

Once you have your Swift package set up, adding DataManager as a dependency is as easy as adding it to the  `dependencies`  value of your  `Package.swift`  or the Package list in Xcode.

```
dependencies: [
    .package(url: "https://github.com/sahibhussain/data-manager.git", .upToNextMajor(from: "1.1.0"))
]
```

Normally you'll want to depend on the  `DataManager`  target:

```
.product(name: "DataManager", package: "DataManager")
```


## Usage
### save
- key: key for the data
- value: value for the data
- isSecure: weather data should be stored in keychain (default: false)
- isFile: weather data should be store in FileManager (default: false)
```
DataManager.shared.save("key", value: "any codable value", isSecure: true, isFile: false)
```

### check if exists
- key: key for the data
- isSecure: weather data is stored in keychain (default: false)
- isFile: weather data is store in FileManager (default: false)
```
DataManager.shared.exist("key, isSecure: true, isFile: false)
```

### retrieve value
- key: key for the data
- isSecure: weather data is stored in keychain (default: false)
- isFile: weather data is store in FileManager (default: false)

```
DataManager.shared.retrieve("key", isSecure: true, isFile: false) 
```

### delete value
- key: key for the data
- isSecure: weather data is stored in keychain (default: false)
- isFile: weather data is store in FileManager (default: false)
```
DataManager.shared.delete("key", isSecure: true, isFile: false) 
```

## Example

### Create a helper enum to make your life easier but is completely optional
```
import DataManager

enum SavedContent: String { 
    case keychain, userDefault, fileManager
	
	private var isSecure: Bool {
		switch self {  
			case .keychain: return true  
			default: return false
		}
	} 

	private var isSavedInDoc: Bool {  
		switch self {  
			case .fileManager: return true
			default: return false  
		}  
	} 

	var exists: Bool { DataManager.shared.exist(rawValue, isSecure: isSecure, isFile: isSavedInDoc) } 
	
	func save<T: Codable>(_ value: T) { DataManager.shared.save(rawValue, value: value, isSecure: isSecure, isFile: isSavedInDoc) }  
	
	func retrieve<T: Codable>() -> T? { DataManager.shared.retrieve(rawValue, isSecure: isSecure, isFile: isSavedInDoc) }
		  
	func delete() {  DataManager.shared.delete(rawValue, isSecure: isSecure, isFile: isSavedInDoc) }
}


// usage
SavedContent.keychain.save("hello world")
let ifExists = SavedContent.keychain.exists
let value = SavedContent.keychain.retrieve()
SavedContent.keychain.delete()
```

# Others

### Contact

Follow and contact me on [X (Twitter)](https://x.com/Sahib_hussain0). If you find an issue, [open a ticket](https://github.com/sahibhussain/data-manager/issues/new). Pull requests are warmly welcome as well.