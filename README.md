# Virus Total System Design
This repository discusses an approach of System Design for a publicly available product like Virus Total

## Functionality covered
* Allows users to upload files to be scanned by multiple AV engines and metadata extraction scripts.
* Engines and scripts may perform a wide range of operations such as: run a proprietary virus scanner, extract metadata from file headers, make calls to external service endpoints, execute uploaded files to observe their behavior, etc.
* Stores metadata and AV engine results about uploaded files. 
Retrieves metadata about uploaded files (including file attributes, metadata and AV engine results).
* Here is an example of metadata collected from file uploads: https://www.virustotal.com/en/file/0bf01094f5c699046d8228a8f5d5754ea454e5e58b4e6151fef37f32c83f6497/analysis/ 
* Ensures all major functionality is available via both a user interface and a public-facing APIs that users can build apps on top of.

## System requirements and constraints
●	The system will support millions of users per day.
●	Uploaded files will range in size from 100K to 1GB.
●	Uploaded files will be retained as long as possible (ideally forever).
●	The system provide results as close to real-time as possible.
●	Metadata and scanning services can run on a mixture of Linux or Windows (not all scanning services support the same OS).
●	Design will be able to accommodate the addition of new AV engines or metadata scripts.  These additions should be as minimally invasive to the running system as possible (i.e. minimal or zero system downtime, no “big bang” deployments, etc.).
