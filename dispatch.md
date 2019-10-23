---
layout: default
title: "Dispatch"
description: A Swift 5.1 reference guide for Dispatch, covering running code on a background thread, main thread, and a practical example.
---

**Dispatch** is a framework in Foundation that allows you to run code on different threads. By default, all code on iOS is run on the _main thread_ (also known as the _UI thread_).

* TOC
{:toc}

### Run code on a background thread

Time-intensive work can run on a **background** thread.[^1]

```swift
import Foundation

// Code in the main thread
DispatchQueue.global(qos: .background).async {
  // Some code that takes a long time to execute
}
```

### Run code on the main thread from a background thread

When background thread work done, the UI can be updated on the **main thread**.[^2]

```swift
import Foundation

// Code in the main thread
DispatchQueue.global(qos: .background).async {
  // Some code that takes a long time to execute
  DispatchQueue.main.async {
    // More code in the main thread
  }
}
```

### Practical example

```swift
import Foundation

@objc func didTapOnGenerateImageButton() {
  DispatchQueue.global(qos: .background).async {
  let newImage = ImageManager.generateImage() // This could take a long time
  DispatchQueue.main.async {
    self.imageView = newImage // The UI can only be updated on the main thread
  }
}
```

### Further reading

* [Dispatch documentation](https://developer.apple.com/documentation/dispatch)

### Notes

[^1]: If a very long function that takes 5 seconds is run on the main thread, the app freezes for 5 seconds, and the user cannot use the app.
[^2]: This way, for the 5 seconds it takes for the function to run, the app UI does not freeze.