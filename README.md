# PyMMB

Python helper functions and minimal example for the data layout underlying the [MultiModalBrowser](https://github.com/platybrowser/mmb-fiji) (MMB), a Fiji plugin for exploring large multi-modal datasets. For the full data in the [PlatyBrowser](https://www.biorxiv.org/content/10.1101/2020.02.26.961037v1), see the [PlatyBrowser backend](https://github.com/platybrowser/platybrowser-backend).

Making the MMB available for custom data is work in progress; any feedback or suggestions are welcome!

## Data layout & Usage

We recommend to store data for the MMB in a versioning structur similar to [semantic versioning](https://semver.org/): a version is named `X.Y.Z` where `X` (major version number) is increased if a new data modality is added, `Y` (minor version number) is increased if new data for a given modality is added and `Z` (patch version number) is increased if existing data is updated.
The entry point for the MMB is a root folder that contains the version folders and the file `versions.json` listing the available versions.

See [platy-data](https://github.com/platybrowser/pymmb/tree/master/platy-data) for an example of 
the MMB data layout, using a small subset of the data available in the PlatyBrowser. You can check out the complete PlatyBrowser layout [here](https://github.com/platybrowser/platybrowser-backend/tree/master/data).

### Version folder

The folder for a given version follows this structure:
```
+images
|  +--images.json
|  +--local
|  +--remote
+misc
+tables
+README.txt
```

- [images/images.json](https://github.com/platybrowser/pymmb#imagesjson): json which lists the avaialable image data and associated display options
- [images/local](https://github.com/platybrowser/pymmb#supported-data-formats): metadata for image data stored locally, in bdv.xml data format
- [images/remote](https://github.com/platybrowser/pymmb#supported-data-formats): metadata for image data stored remotely in a s3 compatible cloud store, in bdv.xml data format
- misc: miscellaneous data, including [bookmarks](https://github.com/platybrowser/pymmb#bookmarksjson)
- [tables](https://github.com/platybrowser/pymmb#supported-data-formats): tabular data associated with image data that has object ids, e.g. segmentations
- README.txt: description of this version, optional


### Supported data formats

The MMB uses [BigDataViewer](https://imagej.net/BigDataViewer) to browse volumetric image data stored locally or on a s3 compatible cloud object store.
Hence the data must be provided in one of these three formats:
- [bdv.hdf5](https://imagej.net/BigDataViewer#About_the_BigDataViewer_data_format) for local data
- [bdv.n5](https://github.com/bigdataviewer/bigdataviewer-core/blob/master/BDV%20N5%20format.md) for local data
- [bdv.n5.s3](https://github.com/saalfeldlab/n5-aws-s3) for data stored on a s3 compatible object store. Note that this is not part of the official bdv spec yet, [we are working towards merging it](https://github.com/bigdataviewer/bigdataviewer-core/issues/80)

Tables are stored as `tab separated values` and can be read from the filesystem or a githost.
For each image with associated tables, the tables are stored in `tables/<IMAGE-NAME>/`.
This folder must contain a table called `default.csv`, it can contain additional tables listed in a file `additional_tables.txt`. All tables must contain the column `label_id` linking its rows to objects in the image.

We are looking into supporting tables hosted via [CouchDB](https://couchdb.apache.org/) as well.


### images.json

TODO describe images.json


### bookmarks.json

TODO describe bookmarks.json


### CustomBrowser

Any data stored in the layout described above can be loaded with the MMB by [selecting the `CustomBrowser` option](https://github.com/platybrowser/mmb-fiji#advanced-options).
For both `Image Data Location` and `Table Data Location` either a local file path or a githost webaddress can be given.
E.g. `https://raw.githubusercontent.com/platybrowser/pymmb/master/platy-data` for accessing the example data in this repository.

## Current limitations

There are a few limitations to use the MMB for custom data, we will try to fix this as soon as possible!

- Image data cannot be loaded from webaddress in `MMB->CustomBrowser`, [see this issue](https://github.com/platybrowser/mmb-fiji/issues/75).
- The modality names are hard-coded to `sbem-6dpf-1-whole` and `prospr-6dpf-1-whole`, which are the names for the modalities currently available in the PlatyBrowser.

<!---
## Installation
-->

## Citation

If you use the MMB for your research, please cite [Whole-body integration of gene expression and single-cell morphology](https://www.biorxiv.org/content/10.1101/2020.02.26.961037v1).
