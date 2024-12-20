# FramelessHelper (Re-Organized)

## Features

- Frameless but have frame shadow.
- Drag and resize.
- High DPI scaling.
- Multi-monitor support (different resolution and DPI).
- **Windows platform**: Acts like a normal window, with animations when minimizing and maximizing, support for tile windows, etc.

---

## Usage

### Windows

```cpp
// include other files ...
#include "winnativeeventfilter.h"
// include other files ...

// Anywhere you like, we use the main function here.
int main(int argc, char *argv[]) {
    // ...
    QWidget widget;
    // Do this before the widget is shown.
    WinNativeEventFilter::addFramelessWindow(reinterpret_cast<HWND>(widget.winId()));
    widget.show();
    // ...
}
```

### Linux and macOS

```cpp
// include other files ...
#include "framelesshelper.h"
// include other files ...

// Anywhere you like, we use the main function here.
int main(int argc, char *argv[]) {
    // ...
    QWidget widget;
    FramelessHelper helper;
    // Do this before the widget is shown.
    // Only QWidget and QWindow are supported.
    helper.setFramelessWindows({&widget});
    widget.show();
    // ...
}
```

---

## Notes

- The `setFramelessWindows` / `addFramelessWindow` function **must not be called after the widget is shown**. However, for **QWindows**, it must be called **after** they are shown.
- `startSystemMove` and `startSystemResize` are used for moving and resizing frameless windows on UNIX platforms. These functions are only available in **Qt 5.15 and above**. If your Qt version is below that, the code will not compile.
- Any widgets (or Qt Quick elements) in the titlebar area will not be responsive because mouse events are intercepted. Try `setIgnoreAreas` and `setDraggableAreas` if needed.
- **Only top-level windows** can be frameless. Applying this to child windows or widgets will result in unexpected behavior.
- For custom border width, border height, titlebar height, or maximum/minimum window size, use the original numbers. **DPI scaling** is handled automatically by this code.

---

## Supported Platforms

- **Windows 7 ~ 10**
- Should work on **X11, Wayland, and macOS**, but not tested.

---

## Building with CMake

To build this project with CMake, follow these steps:

1. Install **Qt6** and **CMake** on your system.
2. Clone the repository.
3. Create a build directory and navigate into it:

    ```bash
    mkdir build
    cd build
    ```

4. Run CMake to configure the project:

    ```bash
    cmake ..
    ```

5. Build the project:

    ```bash
    cmake --build .
    ```

---

## License

This project is licensed under the **MIT License**. See the `LICENSE` file for details.

---

## Acknowledgements

This project is based on [bmzp/framelesshelper](https://github.com/bmzp/framelesshelper).

---