# ODIS OIS Qlever deployment

This project uses a set of Docker Compose files and configuration files to deploy a QLever instance for the ODIS graph dataset.

## File Descriptions

*   `Qleverfile`: This is the main configuration file for the QLever instance. It specifies:
    *   The name of the dataset (`odis`).
    *   The base URL from where to download the data files.
    *   A list of the n-quads files (`.nq`) to be downloaded and indexed.
    *   Server settings such as port, memory limits, and timeouts.

*   `Qleverfile-ui.yml`: This file configures the QLever user interface. It defines:
    *   The connection to the backend QLever server.
    *   Extensive options for SPARQL autocompletion and query suggestions to aid users in building queries.
    *   Example queries.
    * Setting of interest
      * base URL
      * mapViewBaseURL

*   `initialize_compose.yaml`: This Docker Compose file is used for the initial setup and data loading process. It defines a series of services that run in sequence to:
    *   Download the required data files specified in the `Qleverfile`.
    *   Set up the SQLite database for the QLever UI.
    *   Index the data to be used by the QLever engine.
    *   Report on the status of the data loading process.

*   `server_compose.yaml`: This Docker Compose file runs the main QLever services:
    *   `qlever.server`: The core QLever query engine.
    *   `qlever.ui`: A web-based user interface for querying the data.
    *   `qlever.petrimaps`: A service for data visualization.



## Settings

The following documents the various points in the files where


###   `Qleverfile`:

The Qleverfile is the default configuration file for Qlever.  It provides the details on what
files to download, index and parameters for running the server.

Detailed documentation for Qlever can be found at [https://github.com/ad-freiburg/qlever](https://github.com/ad-freiburg/qlever).  For this instance the following elements of the Qleverfile
are of interest.

The entries:

```bash
BASE_URL          = http://ossapi.oceaninfohub.org/public/graphv3
DATA_FILES        = acma_release.nq,africaioc_release.nq,aquadocs_release.nq,argovis_release.nq,bebop_release.nq,benguelacc_release.nq,bmdc_release.nq,bodc_release.nq,calcofi_release.nq,caribbeanmarineatlas_release.nq,cchdo_release.nq,cioos_release.nq,cioosatlantic_release.nq,edmerp_release.nq,edmo_release.nq,emodnet_release.nq,euroceanevents_release.nq,euroceaninstitutions_release.nq,euroceanorgs_release.nq,euroceanprojects_release.nq,euroceanvessels_release.nq,inanodc_release.nq,incois_release.nq,invemardocuments_release.nq,invemarexperts_release.nq,invemargeo_release.nq,invemarinstitutions_release.nq,invemartraining_release.nq,invemarvessels_release.nq,isa_release.nq,marcobolo_release.nq,marineie_release.nq,marinetraining_release.nq,maspawio_release.nq,medin_release.nq,metsrcn_release.nq,mims_release.nq,ncei_release.nq,obis_release.nq,obps_release.nq,oceanexpert_release.nq,oceanexperts_release.nq,oceanscape_release.nq,odiscat_release.nq,openasfa_release.nq,osmc_release.nq,pdh_release.nq,pedp_release.nq,r2r_release.nq,rda_release.nq,spase_release.nq,unep_release.nq,wiosymphony_release.nq,wod_release.nq,zmt_release.nq
```

define the location of the files.  Here the URL for the exposed graphs from the OIH object store are provided via
a combination of BASE_URL and then the individual DATA_FILES.

For the server, this is also where the port number will be set with:

```bash
PORT               = 7007
```

While several other elements in this file like DESCRIPTION and FORMAT are important, the defaults can be used
for now.


###   `Qleverfile-ui.yml`:

This configuration file is for the UI.  The main thing the UI needs to know is the location of the
SPARQL endpoint and the map server.  These are set with

```yaml
    baseUrl: http://localhost:7007
```

```yaml
    mapViewBaseURL: "http://localhost:9090"
```

update these for your instance.

### `initialize_compose.yaml`:

The only item in this document you might change is the name of the data volume.  The default is ql_dvol.

###  `server_compose.yaml`:

While you can change things like container names or other elements it is likely only the port values you would
want to change.  By default the SPARQL endpoint will be exposed on 7007, the UI will be on port 8176 and the map
interface will be on port 9090.

The value for `QLEVER_HOST: "http://localhost:7007"`  should also be changed to the hostname and port you
defined.


## Usage

1.  **Initialization:** Run `docker-compose -f initialize_compose.yaml up` to download and index the data.
2.  **Start Server:** Once the initialization is complete, run `docker-compose -f server_compose.yaml up` to start the QLever server and UI.

By default:

The QLever SPARQL endpoint will be available at `http://localhost:7007`.

The QLever UI will be available at `http://localhost:8176`.

## Running outside of Docker



## Snipits


To remove all containers and geenrated volumes (note volume name will change based on execution directory).


```bash
docker compose -f server_compose.yaml down ;  docker compose -f initialize_compose.yaml down ;  docker volume rm  test_oih_ql_dvol
```
