# Thesios: Google Synthesized I/O Traces for Storage Servers and Disks

This repository describes I/O traces of Google storage servers and disks
synthesized by Thesios. Thesios synthesizes representative I/O traces by
combining down-sampled I/O traces collected from multiple disks (HDDs) attached
to multiple storage servers in Google distributed storage system.

Please refer to our [paper](https://dl.acm.org/doi/10.1145/3620666.3651337) for
more details on the synthesis methodology, validation, and example use cases. If
you use this data in your research, please cite:

```
@inproceedings{thesios,
  author = {Phothilimthana, Phitchaya Mangpo and Kadekodi, Saurabh and Ghodrati, Soroush and Moon, Selene and Maas, Martin},
  title = {Thesios: Synthesizing Accurate Counterfactual I/O Traces from I/O Samples},
  year = {2024},
  isbn = {9798400703867},
  publisher = {Association for Computing Machinery},
  url = {https://doi.org/10.1145/3620666.3651337},
  doi = {10.1145/3620666.3651337},
  series = {ASPLOS '24}
}
```

## Data Description

The data includes I/O traces from 2024/01/15 to 2024/03/15 from three different
clusters (with different types of traffic). A one-day trace is stored as sharded
CSV files, named `{cluster}_{disk_size}/{yyyymmdd}/data*`. Each trace contains
I/O requests to a storage server with a single disk (HDD) attached.

| Field                            | Description                               |
| -------------------------------- | ----------------------------------------- |
| *--- collected fields ---*       |                                           |
| filename                         | Local filename (hashed)                   |
| application                      | Application owner of the file (hashed     |
:                                  : except for spanner, bigtable, or          :
:                                  : blobstore)                                :
| file_offset                      | File offset                               |
| c_time                           | Inode change time in Unix seconds since   |
:                                  : epoch                                     :
| io_zone                          | WARM, COLD, or UNKNOWN                    |
| redundancy_type                  | REPLICATED or ERASURE_CODED               |
| op_type                          | READ or WRITE                             |
| service_class                    | Request's priority: LATENCY_SENSITIVE,    |
:                                  : THROUGHPUT_ORIENTED, or ORTHER            :
| from_flash_cache                 | Whether the request is from flash cache   |
| cache_hit                        | Whether the request is served by server's |
:                                  : buffer cache: hit = 1, miss = 0,          :
:                                  : write = -1 (n/a)                          :
| request_io_size_bytes            | Size of the request in bytes              |
| response_io_size_bytes           | Size of the response in bytes             |
| disk_io_size_bytes               | Size of the disk operation in bytes (0    |
:                                  : for cache hit)                            :
| start_time                       | Request's arrival time at the server in   |
:                                  : Unix seconds since epoch                  :
| disk_time                        | Disk read time in seconds (0 for cache    |
:                                  : hit or write)                             :
| *--- synthesized fields ---*     |                                           |
| simulated_disk_start_time        | Start time of disk read in Unix seconds   |
: (disk-level)                     : since epoch (0 for cache hit or write)    :
| simulated_latency (server-level) | Latency in seconds from arrival time to   |
:                                  : response time at the server. For cache    :
:                                  : miss read, the value is adjusted by a     :
:                                  : simulator. For cache hit or write, the    :
:                                  : value is from real measurement from the   :
:                                  : original trace.                           :

### Disclaimers

1. The synthesized traces do not include I/O requests to archival files stored
in the innermost partition of a disk. However, the number of requests to
archival files is negligible compared to accesses to non-archival files.
2. The disk size in the folder name is just one instance of one disk type
in the cluster.
3. The I/O that is absorbed by the flash caching layer is not included in this
trace.


## Download

The dataset is located at https://console.cloud.google.com/storage/browser/thesios-io-traces.
You can use `gcloud`, `wget`, `curl` command line utilities to download files,
or directly download from the webpage.


## CC-BY License

This work is licensed under the Creative Commons Attribution 4.0 International
License. To view a copy of this license, visit
http://creativecommons.org/licenses/by/4.0/ or send a letter to Creative
Commons, PO Box 1866, Mountain View, CA 94042, USA.

