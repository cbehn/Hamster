# Rhino3D Networked Parts Library

A Rhino3D plugin to manage and insert reusable 3D parts (as blocks with metadata) from a shared network database.

## Project Goal

To create a robust and user-friendly system for designers and engineers to quickly find, access, and insert standardized 3D parts into their Rhino projects. The library will be centrally managed and accessible across a local area network.

## Requirements

### Functional Requirements:
1.  **Centralized Database:**
    *   The database storing part information (metadata, path to geometry file) must be accessible from multiple Rhino instances on a network.
    *   The 3D part files themselves (e.g., `.3dm` files) will also be stored in a shared network location.
2.  **Rhino Plugin Interface:**
    *   A user-friendly panel or dialog within Rhino.
    *   Browse and search capabilities for parts based on metadata (e.g., part name, ID, category, description).
    *   Preview (thumbnail or interactive) of the selected part before insertion (optional for early milestones).
3.  **Part Insertion:**
    *   Selected parts must be inserted into the active Rhino document as block instances.
    *   Relevant metadata from the database should be attached to the inserted block instance (e.g., as User Text or attributes).
4.  **Library Management (Admin/Librarian Features):**
    *   Ability to add new parts to the library:
        *   Specify/upload the `.3dm` geometry file.
        *   Enter/edit metadata for the part.
    *   Ability to edit metadata of existing parts.
    *   Ability to delete parts from the library (with appropriate warnings/confirmations).
5.  **Metadata:**
    *   Each part should have associated metadata, including but not limited to:
        *   Part Name / Identifier
        *   Description
        *   Category / Type
        *   Source / Manufacturer
        *   Material
        *   Path to the geometry file
        *   Thumbnail image path (optional)

### Non-Functional Requirements:
1.  **Usability:** Intuitive and efficient for users to find and insert parts.
2.  **Performance:** Fast search and loading of parts, especially for large libraries.
3.  **Reliability:** Robust handling of network connections and file access.
4.  **Scalability:** The system should be able to handle a growing number of parts and users.

## Tech Stack

1.  **Rhino Plugin Development:**
    *   **Language:** Python (utilizing `RhinoScriptSyntax` and `RhinoCommon` API).
    *   **UI Framework:** Eto Forms (for creating a cross-platform user interface within Rhino).
2.  **Database:**
    *   **Type:** Relational Database.
    *   **Initial Choice:** SQLite.
        *   *Reasoning:* Simple to set up (file-based), can be placed on a network share. Good for initial development and smaller teams.
    *   **Alternative/Scalable Options:** PostgreSQL or MySQL.
        *   *Reasoning:* More robust for concurrent multi-user access, better performance for very large datasets, requires a dedicated server.
3.  **Part File Storage:**
    *   Standard Network File Share (e.g., SMB/CIFS). Part geometry will be stored as individual `.3dm` files.
4.  **Data Exchange (if a dedicated DB server like PostgreSQL/MySQL is used):**
    *   Standard Python database connectors (e.g., `psycopg2` for PostgreSQL, `mysql.connector` for MySQL). `sqlite3` is built into Python.

## Milestones (Up to 5)

1.  **M1: Proof of Concept - Local Block Insertion with Metadata:**
    *   Develop a basic Rhino Python script that can:
        *   Define a part (hardcoded file path to a `.3dm` file and basic metadata).
        *   Insert this part as a block into the current Rhino document.
        *   Attach the hardcoded metadata to the inserted block instance (e.g., as UserText).
    *   *Goal: Validate core Rhino block creation and metadata attachment.*

2.  **M2: Basic Database & Plugin UI Shell:**
    *   Design and create the SQLite database schema (e.g., `parts` table with columns for id, name, description, file_path, category).
    *   Develop a simple Eto UI panel within Rhino that can:
        *   Connect to the (initially local) SQLite database.
        *   List parts fetched from the database.
        *   Implement functionality to insert a selected part from the list as a block (reusing M1 logic).
    *   *Goal: Establish database connection, basic UI, and dynamic part listing/insertion.*

3.  **M3: Part Management & Networked Database:**
    *   Implement UI functionality for adding new parts to the library:
        *   Dialog to select a `.3dm` file.
        *   Fields to input metadata.
        *   Logic to copy the `.3dm` file to a designated library folder and add its record to the database.
    *   Configure the system to use the SQLite database and part files from a shared network location.
    *   Test basic access from multiple machines (if feasible).
    *   *Goal: Enable library population and test network accessibility.*

4.  **M4: Enhanced Search, Filtering & Metadata Handling:**
    *   Implement search functionality in the UI to filter parts based on metadata fields (name, category, etc.).
    *   Refine how metadata is stored on block instances (ensure it's comprehensive and easily accessible via Rhino tools or scripts).
    *   (Optional Stretch Goal: Implement simple thumbnail generation/display for parts in the browser).
    *   *Goal: Improve usability for finding parts and ensure metadata integrity.*

5.  **M5: Refinement, Error Handling, and Basic Documentation:**
    *   Improve error handling (e.g., database connection issues, missing part files).
    *   Add user feedback mechanisms (status messages, confirmations).
    *   Conduct testing with a wider range of parts and scenarios.
    *   Create basic user instructions on how to use the plugin and manage the library.
    *   *Goal: Create a more robust and user-friendly tool ready for initial deployment.*
