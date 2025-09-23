
# Calling C++ DLLs from C#

You can call functions from a C++ DLL in C# using **P/Invoke (Platform Invocation Services)**. Here’s a step-by-step example.

### 1. Create the C++ DLL

Suppose you have a simple C++ DLL project (compiled as `MyNativeLib.dll`):

```cpp
// MyNativeLib.cpp
#include <windows.h>

extern "C" __declspec(dllexport) int AddNumbers(int a, int b)
{
    return a + b;
}

extern "C" __declspec(dllexport) const char* GetMessage()
{
    return "Hello from C++ DLL!";
}
```

* `extern "C"` ensures the functions are exported without C++ name mangling.
* `__declspec(dllexport)` makes the functions visible outside the DLL.

Compile this as a DLL (e.g., `MyNativeLib.dll`).

---

### 2. Import the DLL into C\#

In your C# project:

```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    // Import the functions from the C++ DLL
    [DllImport("MyNativeLib.dll", CallingConvention = CallingConvention.Cdecl)]
    public static extern int AddNumbers(int a, int b);

    [DllImport("MyNativeLib.dll", CallingConvention = CallingConvention.Cdecl)]
    private static extern IntPtr GetMessage();

    static void Main()
    {
        // Call the C++ function
        int sum = AddNumbers(5, 7);
        Console.WriteLine($"Sum: {sum}");

        // Get string from DLL
        IntPtr ptr = GetMessage();
        string message = Marshal.PtrToStringAnsi(ptr);
        Console.WriteLine(message);
    }
}
```

---

### 3. Build & Run

* Place `MyNativeLib.dll` in the same folder as the C# executable (or in a path accessible via `PATH`).
* Run your C# program, and you’ll see:

```
Sum: 12
Hello from C++ DLL!
```

---

⚠️ Notes:

* Make sure the calling conventions match (`__cdecl` vs `__stdcall`).
* If you’re using x64, ensure both the DLL and C# app target the same architecture (x86/x64).
* For more complex types (like structs), you’ll need to define corresponding C# structures with `[StructLayout]`.

