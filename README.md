# ODIS OIS Qlever deployment

This project uses a set of Docker Compose files and configuration files to deploy a QLever instance for the ODIS graph dataset.

### File Descriptions

*   `initialize_compose.yaml`: This Docker Compose file is used for the initial setup and data loading process. It defines a series of services that run in sequence to:
    *   Download the required data files specified in the `Qleverfile`.
    *   Set up the SQLite database for the QLever UI.
    *   Index the data to be used by the QLever engine.
    *   Report on the status of the data loading process.

*   `server_compose.yaml`: This Docker Compose file runs the main QLever services:
    *   `qlever.server`: The core QLever query engine.
    *   `qlever.ui`: A web-based user interface for querying the data.
    *   `qlever.petrimaps`: A service for data visualization.

*   `Qleverfile`: This is the main configuration file for the QLever instance. It specifies:
    *   The name of the dataset (`odis`).
    *   The base URL from where to download the data files.
    *   A list of the n-quads files (`.nq`) to be downloaded and indexed.
    *   Server settings such as port, memory limits, and timeouts.

*   `Qleverfile-ui.yml`: This file configures the QLever user interface. It defines:
    *   The connection to the backend QLever server.
    *   Extensive options for SPARQL autocompletion and query suggestions to aid users in building queries.
    *   Example queries.

### Usage

1.  **Initialization:** Run `docker-compose -f initialize_compose.yaml up` to download and index the data.
2.  **Start Server:** Once the initialization is complete, run `docker-compose -f server_compose.yaml up` to start the QLever server and UI.

The QLever UI will be available at `http://localhost:8176`.
