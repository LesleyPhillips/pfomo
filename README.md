"# pfomo" 

A terminal to have some old school fun.   : )

Working with Github AI  I added remarks to all the code.
Documenting the following
1. HTML Structure - DOCTYPE, head, meta tags, and title
2. CSS Styles - All style rules for body, terminal, cursor animation
3. JavaScript Variables - Version, DOM elements, state variables
4. Screen Buffer System - 30x80 character grid simulation
5. Terminal API - All methods for screen manipulation
6. Core Functions:
   - addInputLine() - Prompt setup
   - clearScreen() - Screen reset
   - getLineText() - Text extraction
   - printToTerminal() - Output rendering
   - scroll() - Screen scrolling
   - setLineText() - Line manipulation
   - render() - HTML conversion and display
   - writeAt() - Character writing
7. Input Handling:
   - executeCommand() - Command execution
   - handleKey() - Keyboard events (Enter, Backspace, Arrow keys, printable chars)
   - Mobile input handlers
8. Commands:
   - Command registry system
   - All command implementations (cls, dir, echo, help, version, whoami, whoislesley, drawcorners, drawrandomblocks)
   - Initialization - Async startup sequence


PFOMO.COM Explained

PFOMO - A Retro Terminal Emulator   
---------------------------------

This project is a nostalgic terminal emulator that recreates the experience of classic command-line interfaces directly in your web browser. The name "PFOMO" suggests it's designed to bring back some "old school fun" while working as a fully functional terminal environment.

Project Overview   -----

The application simulates a vintage terminal experience through careful attention to detail in three main areas: the HTML structure provides the foundation, CSS styling creates the authentic retro aesthetic (likely including elements like monospace fonts and possibly scan line effects), and JavaScript powers all the interactive functionality.

Architecture and Core Systems   -----

At the heart of the application is a screen buffer system that emulates a traditional 30x80 character grid—the standard dimensions of many classic terminals. This buffer acts as a virtual representation of the screen, allowing the terminal to manage text positioning, scrolling, and updates just like hardware terminals did decades ago.

The Terminal API exposes a set of methods that handle all screen manipulation tasks. These methods abstract away the complexity of working directly with the DOM, making it easier to write commands and manage terminal output programmatically.

Key Functionality   -----

Several core functions work together to create the terminal experience:

- addInputLine() sets up the command prompt where users type
- clearScreen() wipes the display clean
- printToTerminal() handles outputting text to the screen
- scroll() moves content upward when the screen fills up
- writeAt() places characters at specific grid positions
- render() converts the internal buffer into HTML for display

User Interaction   -----

The input handling system captures keyboard events and routes them appropriately. When you press Enter, executeCommand() processes your input. Backspace removes characters, arrow keys navigate through command history (a typical terminal feature), and all printable characters appear on screen. The application even includes mobile input handlers, making it accessible on touch devices.

Command System   -----

PFOMO includes a command registry that maps command names to their implementations. Built-in commands include familiar utilities like cls (clear screen), dir (directory listing), echo (print text), help (show available commands), and version (display version info). There are also some fun additions like whoami, whoislesley (possibly an Easter egg about the developer), drawcorners, and drawrandomblocks that demonstrate the terminal's graphical capabilities within its character grid constraints.

Initialization   -----

The application uses an async startup sequence to initialize everything properly. This asynchronous approach ensures that all components load correctly before the terminal becomes interactive, providing a smooth user experience from the moment the page loads.

This project represents a thoughtful blend of modern web technologies with retro computing aesthetics, creating an educational and entertaining environment for exploring command-line interfaces.


PFOMO.com Flow Described

Code Flow Overview
------------------

1. Page Load & Initialization   -----

When the HTML loads, the browser processes everything in order:

HTML structure is parsed (terminal div, hidden mobile input)
CSS styles are applied (green-on-black terminal aesthetic, cursor animation)
JavaScript begins executing

2. Variable Declaration Phase   -----

The script sets up its environment:

Gets references to DOM elements (terminal, mobileInput)
Initializes empty command history array
Detects if user is on mobile device
Creates the 30x80 character screen buffer (30 arrays of 80 spaces each)
Sets cursor position to (0,0)
Defines the Terminal API object that exposes all screen manipulation methods

3. Function Definitions   -----

All functions are defined but not yet executed:

Screen manipulation functions (clearScreen, scroll, render, etc.)
Input handling (handleKey, executeCommand)
Command implementations (cmdCls, cmdEcho, cmdHelp, etc.)
Command registry system

4. Event Listener Registration   -----

The script attaches event handlers:

window.addEventListener('keydown', handleKey) - captures all keyboard input
terminal.addEventListener('click', ...) - focuses mobile input on tap
terminal.addEventListener('touchstart', ...) - handles touch events
If mobile: additional handlers for the hidden input field

5. Command Registration   -----

Each command is registered in the commandRegistry object:

   registerCommand('cls', fn, helpText)
   registerCommand('echo', fn, helpText)
   // ... etc

6. Async Startup (IIFE)   -----

The immediately-invoked async function executes:

   (async () => {
   await initializeGlobalVars();  // Fetches user IP from API
   clearScreen();                  // Writes welcome message to buffer
   cursorY++;                      // Adds blank line
   addInputLine();                 // Draws prompt '>' and enables typing
   })();

At this point, the terminal is ready and waiting for user input.

User Interaction Flow
---------------------

When User Types a Character:

1. handleKey(event) receives the keydown event
2. Checks if typingEnabled is true (it is, after prompt)
3. Checks it's not a modifier key
4. If printable character (length === 1):
   - Calls writeAt(cursorX, cursorY, char) to place character in buffer
   - Increments cursorX
   - Checks for line wrap (if cursorX >= 80)
   - Checks for scroll (if cursorY >= 30)
   - Calls render() to update the display

When User Presses Enter:

1. handleKey(event) detects Enter key
2. Extracts command text via getLineText(cursorY)
3. Sets typingEnabled = false to prevent input during execution
4. Adds command to commandHistory array
5. Moves cursor to next line
6. Calls executeCommand(command):
   - Splits command into parts
   - Looks up command in commandRegistry
   - If found: executes the command function with args
   - If not found: prints "UNKNOWN COMMAND"
7. Command function (e.g., cmdEcho()) calls printToTerminal():
   - Iterates through each character
   - Calls writeAt() for each character
   - Handles newlines by incrementing cursorY
   - Auto-wraps at column 80
   - Auto-scrolls at row 30
   - Calls render() to display output
8. After command completes, moves cursor to next line
9. Calls addInputLine() to show new prompt and re-enable typing

Rendering Flow
--------------

Every time render() is called:

1. Creates empty lines array
2. Loops through all 30 lines in screenBuffer
3. For the cursor line:
   - Loops through all 80 characters
   - Wraps cursor position in <span class="cursor"> for blinking effect
   - Escapes HTML characters
   - Converts spaces to &nbsp;
4. For non-cursor lines:
   - Detects URLs with regex and wraps in temporary markers
   - Escapes HTML
   - Converts URL markers to clickable <a> tags
5. Joins all lines with newlines
6. Wraps in <pre> tag with styling
7. Sets terminal.innerHTML to the generated HTML
8. Auto-scrolls to bottom

Special Cases
-------------

Backspace Flow:

- Checks cursor is past column 1 (can't delete prompt)
- Moves cursor left
- Writes space to erase character
- Renders

Arrow Up/Down Flow:

- Arrow Up: moves backward through commandHistory, displays previous command
- Arrow Down: moves forward, or clears line if at end
- Updates line via setLineText() and positions cursor at end

Mobile Input Flow:

- User taps terminal → focuses hidden input field → keyboard appears
- Characters typed → input event fires → writes to buffer → clears input field
- Enter/Backspace → simulates keyboard events for main handleKey() function

This flow creates a seamless loop: user types → buffer updates → render displays → wait for next input.


