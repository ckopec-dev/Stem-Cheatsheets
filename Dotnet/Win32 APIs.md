
# Calling Win32 APIs from C-Sharp

Call the `MessageBox` function from `user32.dll`.

```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    // Import the MessageBox function from user32.dll
    [DllImport("user32.dll", CharSet = CharSet.Auto)]
    private static extern int MessageBox(IntPtr hWnd, String text, String caption, uint type);

    static void Main()
    {
        // Call the Win32 API
        MessageBox(IntPtr.Zero, "Hello from Win32 API!", "Win32 API Call", 0);
    }
}
```

## Explanation

1. **`[DllImport]`** – Attribute tells the runtime we’re importing an unmanaged function from a DLL.
2. **`user32.dll`** – Contains Windows user interface APIs.
3. **`MessageBox`** – Displays a message box with text and caption.
4. **`IntPtr.Zero`** – No parent window (desktop).
5. **`type`** – Flags for message box buttons and icons (here `0` = OK button only).

---

### Example 1 – Calling `GetTickCount`

This function returns the number of milliseconds since Windows started.

```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    // Import GetTickCount from kernel32.dll
    [DllImport("kernel32.dll")]
    private static extern uint GetTickCount();

    static void Main()
    {
        uint uptime = GetTickCount();
        Console.WriteLine($"System uptime: {uptime / 1000} seconds");
    }
}
```

---

### Example 2 – Calling `Beep`

This function makes the PC speaker beep at a given frequency and duration.

```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    // Import Beep from kernel32.dll
    [DllImport("kernel32.dll")]
    private static extern bool Beep(uint frequency, uint duration);

    static void Main()
    {
        Console.WriteLine("Playing a beep...");
        Beep(1000, 500); // 1000 Hz for 500 ms
    }
}
```

---

✅ Both of these show:

* Importing functions from `kernel32.dll` (system-level functions).
* Returning values (`uint` for `GetTickCount`, `bool` for `Beep`).

