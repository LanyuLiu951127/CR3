# GEMINI.md

## Project Overview

This project is a Python desktop application designed to help photographers find duplicate Canon CR3 raw image files. The application provides a graphical user interface (GUI) to select a folder, and then it analyzes the CR3 files to identify duplicates, grouping them for easy review.

The project is structured as a Python application. The main logic is intended to be in `CR3/CR3/main.py`, although the current implementation is just a placeholder. The application relies on the `Pillow` library for image processing.

## Building and Running

### Prerequisites

*   Python 3.6+
*   `pip` for installing packages

### Installation

1.  **Install Dependencies:** The project requires the `Pillow` library.

    ```bash
    pip install Pillow
    ```

### Running the Application

To run the application, navigate to the `CR3/CR3` directory and execute the main Python script:

```bash
cd CR3/CR3
python main.py
```

**TODO:** The `main.py` file currently only prints a "Hello World" message. The core logic for the GUI, file selection, and duplicate detection needs to be implemented.

## Development Conventions

*   The project is written in Python.
*   The application should have a graphical user interface.
*   The core functionality is to identify and group duplicate CR3 files.
*   The project structure suggests that documentation is kept in the `doc` directory.
