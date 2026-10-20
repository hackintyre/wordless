# wordless
Immersive Offline Word Processor

Wordless 1.0.6
Wordless is an offline-first desktop writing app. DOCX remains the primary editable document format.
Features
DOCX editing: basic text formatting, headings, alignment, and local document saving.
Simple workspace: removed the Insert and Layout ribbon tabs, Read Mode, and nonfunctional page margin/page size controls for this 1.0 release. Printing is available from the File tab.
PDF reading: choose a PDF from File > Open. PDF pages open in a separate read-only viewer; Wordless does not edit PDFs. PyMuPDF and Pillow are included in requirements for the built-in viewer.
Persistent recent files: recent DOCX, text, and PDF paths are stored locally in the Wordless configuration directory.
Themes: Autumn (pixel leaves), Galaxy (white star clusters), Enchanted Forest (fireflies), Snowy (falling snow), and Retro (dark purple and pink neon with vertical Matrix-inspired binary streams).
Personal background: use Themes… to choose a local photo and adjust the overall backdrop and theme blends. The selected path and settings persist across restarts. The photo is blended into the backdrop and side gutters; writing text remains on a solid high-contrast surface for legibility.
Writing Companion: offline prompt templates and revision checklists. These do not use a cloud AI service.
Build on Windows
Install Python 3.10+ and ensure the Python launcher (`py`) is available.
Run `build_windows.bat`.
The dependency checker reports which required packages are already compatible and installs only missing or incompatible packages. It no longer upgrades pip on every run.
Launch `dist\Wordless.exe`.
Version: 1.0.4. The build uses PyInstaller's `--collect-all pymupdf` option to include PyMuPDF's package resources, including the layout YAML file that some newer releases require at startup. If that resource was previously omitted from the executable, rebuild using this updated script.
Local settings and privacy
Wordless stores recent file paths and theme/background settings locally under `%APPDATA%\Wordless` on Windows, or `~/.config/Wordless` on Linux/macOS. The selected background photo stays at its original local path; Wordless does not upload it.
Limitations
DOCX formatting is supported, but Wordless is not a complete Microsoft Word replacement and may not preserve every advanced DOCX feature.
PDF is view-only. PDF annotations, form editing, OCR, and PDF export are not supported. If the PDF library cannot initialize, Wordless still starts and delegates PDF opening to the operating system's default viewer.

Version 1.0.3
Autumn document outline text uses larger cream-colored labels for contrast.
The Home landing page follows the currently selected theme instead of retaining Retro colors.
Renamed the recent-files section to JUMP BACK IN.
The landing greeting randomly cycles through ten short greetings at startup.

Version 1.0.3
Improved Autumn contrast for font controls, document outline, and Themes button.
Added a reset action in Themes to clear the personal background image and restore default background blend settings.
Resetting applies the selected theme’s standard colors.
Updated landing-page footer text to follow the active theme.
Version 1.0.6
Removed the decorative subtitle above the rotating welcome greeting.
Rebuilds recent-file rows and separators on theme changes to remove stale Retro-purple dividers from other themes.
Improved Autumn outline-link contrast.

Wordless 1.0.6
Improved Autumn theme contrast in the Themes window, including slider labels and background path text.
Snowy Writing Companion now uses dark text on light surfaces for readability.
Snow animation now uses small, fluffy pixel-art snow clusters.
