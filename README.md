# SWS-Compatible-Subclassing
Reaper uses ANSI windows based on SWELL, which extends parts of the Win32 API to MacOS and Linux; however, there are limitations remedied by libraries developed for Reaper, such as SWS. SWS-Compatible-Subclassing will provide true multi-user extendibility of window procedure subclassing that remains compatible with SWS, SWELL, and thus Reaper.

# ANSI Backwards Compatibility
The fact that reaper uses ANSI windows poses a challenge to using UNICODE only Windows API features, such as SetWindowSubclass. The commctrl library SetWindowSubclass and RemoveWindowSubclass are the standard safe way of subclassing window procedures, but it only works on UNICODE windows, which creates a problem for writing Reaper extensions. If each extension wants to properly subclass, then there is no agreement on how it should be done if they cant use SetWindowSubclass and RemoveWindowSubclass; if one extension removes its leading window procedures through functions like SetWindowLongPtr, then the chain breaks. The issue is an example of why its not considered safe to use SetWindowLongPtr unless you are the original author, and why Windows chose to implement SetWindowSubclass, which is intentionally designed to work with random users by maintaining a window property for its call chain. In SWS-Compatible-Subclassing, all the funcions are developed explicitely for ANSI window subclassing in Reaper for non-authors. 

# Target Platforms
- Windows x86
- Windows x86_64
- MacOS x86 (maybe)
- MacOS x86_64
- MacOS aarch64
- Linux x86
- Linux x86_64
- Linux ARM
- Linux aarch64

# Features (only work for ANSI window procedures and ANSI windows, otherwise behavior is undefined)
- bool Goschie_SetWindowSubclassA(HWND hwnd, SUBCLASSPROC pfnSubclassA, UINT_PTR uIdSubclassA, DWORD_PTR dwRefData)
- bool Goschie_RemoveWindowSubclassA(HWND hwnd, SUBCLASSPROC pfnSubclassA, UINT_PTR uIdSubclassA)
- bool Goschie_GetWindowSubclassA(HWND hwnd, SUBCLASSPROC pfnSubclassA, UINT_PTR uIdSubclassA, DWORD_PTR* pdwRefData)
- LRESULT Goschie_DefSubclassProcA(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)

# bool Goschie_SetWindowSubclassA(HWND hwnd, SUBCLASSPROC pfnSubclassA, UINT_PTR uIdSubclassA, DWORD_PTR dwRefData)
Adds ANSI window procedure pfnSubclassA to front of call chain on ANSI window using hwnd. uIdSubclassA and pfnSubclassA together describe a unique identifier (uIdSubclassA can be any value), if the unique identifier already exists, then dwRefData is set to a new value. dwRefData is typically an address to persistent data. 
- The SetWindowSubclassA procedure permanently heads the call chain in a thunk; this makes call chain removal compatible with SetWindowLongPtr and SetWindowLong.

# bool Goschie_RemoveWindowSubclassA(HWND hwnd, SUBCLASSPROC pfnSubclassA, UINT_PTR uIdSubclassA)
Removes ANSI window procedure pfnSubclassA from call chain on ANSI window using hwnd. uIdSubclassA and pfnSubclassA describe a unique identifier (uIdSubclassA is original value set with SetWindowSubclassA).

# bool Goschie_GetWindowSubclassA(HWND hwnd, SUBCLASSPROC pfnSubclassA, UINT_PTR uIdSubclassA, DWORD_PTR* pdwRefData)
Retrieves last dwRefData value in pdwRefData. uIdSubclassA and pfnSubclassA describe a unique identifier (uIdSubclassA is original value set with SetWindowSubclassA).

# LRESULT Goschie_DefSubclassProcA(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)
Calls the next window procedure in the call chain. Typically use this to return from SUBCLASSPROC functions. 
