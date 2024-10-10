# wildlifeRecords2Hdf (Python module)

Python module for summarizing environmental sound (image) and metadata for scientific analysis.

> **Framework with methods for organising mass source data, in particular environmental sound recordings or wildlife camera images, including other recorded data that is directly related to this mass source data, in such a way that the data can be easily accessed at any later point in time. The magic words are "self-describing database".**

**If you are thinking about how you or your workgroup should store a large amount of sound or/and image data for subsequent analysis,** then this module could be the answer. The module also provides methods to work with this data directly with the [opensoundscape](https://opensoundscape.org) module for analysis.

## Typical use cases for which the Python module will be made

### Environmental sound recording

Environmental sound recording is a frequently used application in field research to detect the presence of species in a certain area or to monitor their presence in the long term if they make themselves heard. This is common for the group of birds (also night bird migration), for bats and sometimes for acoustically active insects such as cicadas. In areas where other animal groups, such as monkeys, also frequently make their presence and activity known through vocalizations, these methods can also be used for such animal groups.

A 24/7 recording will yield a lot of sample data. For bird calls and songs as example, up to 12 kHz and 4-fold oversampling is useful, so 48 kS/s and a resolution of 16 bits would be a good choice: The data volume per month is 1/4 TByte. It is commen that recorder systems split the data into smaller chunks to protect it against a variety of error cases, from write errors to power outages. Splitting into 15-minute chunks would give you 86.4 MB per file and 2880 files per month. With two-channel audio for better extraction of simultaneously calling or singing individuals (as a later pre-processing step), it results in half a terrabyte per month and field recorder. This data volume is of course somewhat lower if specialised devices such as AudioMoth are used, which can only record time segments across the day.For 24/7 recording you get a lot of sampling data. For bird calls, as example, until 12kHz and 4 times oversampling, 48kS/s and 16Bit resolution is a good choice. You get 250GB per month and channel. Typical recorder configurations split the data in smaller chunks to be save in different cases from write erros until power-of scenarios. 15 minutes chunks would have 86.4MB per file and 2880 files per month. In the case of 2-channel-audio, for a better individuen extraction as preprocessing, we get a half Terrabyte per Month per field recorder. This amount of data is smaller in case of using soecial devices, like AudioMoth, what can record only time slices over each day.

At the latest when you use several such field recorders and perhaps also want to combine the data with metadata, such as the current weather, the question arises as to how this data can be meaningfully merged and stored. This is especially true if you want to preserve the data for future tasks.At the latest when you use several such field recorders and perhaps also want to combine the data with metadata, such as the current weather, the question arises as to how you can reasonably bring together and store this data

### Additional data acquisition

The objectives of field research can vary considerably, which is why it can be necessary and is common practice to collect additional information alongside the sound data. In the following, this data is called ‘metadata’, although it can be more valuable in terms of content than simply providing context for the sound data. However, as the core of the module is essentially used to summarize sound data, we will continue to use the term ‘metadata’ for this data. This generally and commonly includes weather data, current light brightness, e.g. the actual twilight brightness at the location of the sound recording. Particularly near breeding sites, however, movement data of individuals is also of interest and must be recorded synchronously with the sound and possibly also in parallel with recordings from image recording devices. This means that these metadata are not always just simple attributes of the sound data, but can result in completely independent data series. Although in many cases the metadata actually represent little more than attributes of the sound data, we would at least like to leave open the possibility here that metadata can also be time-stamped information that occurs in parallel to the sound data.

### Pragmatic solutions, their problems and the aim of the project

Not every scientific working group has the opportunity to appoint an experienced software-savvy data specialist as a member of their group. Depending on the experience of the members of the working group, pragmatic solutions are chosen to summarize the data from the data recorders. The primary, minimum goal is to summarize the data in such a way that it can be used for at least a single analysis with subsequent publication of findings. The consequence of this is that the data is lost for subsequent use at a later date or by other working groups if not at least one member of the original working group is not available to help interpret the data files. This is a common and recurring problem in science.

While openly accessible and well-documented software tools are usually used for the actual evaluation of the data, it is rather difficult to access the source data again later and extract it from the collected data series due to the pragmatic isolated solutions described above.

There are now good examples where such mass data series can be stored in a format that provides the desired conditions for reusability of the data. In the field of artificial intelligence, the need for well-structured mass data has become established and has led to solutions such as Pandas, which actually enable the data to be reused beyond the working group that generated it.

However, data sampled in great detail over time, such as environmental audio data, has special requirements, as this data is usually not fed to AI applications as a pure data series, but is selected and pre-processed in pre-processing phases and sometimes converted into completely different mathematical spaces. All the more so if metadata containing more information than a simple singular attribute for a sound data series over a specific time range is also recorded in parallel.

This project is intended to address precisely this specificity in order to ensure the implementation of ‘good scientific practice’ (publication of source data in parallel with the publication of results obtained from it) for scientific data series with the content character described above, but also to provide working groups with limited resources with tools for data structuring in addition to the analysis tools that are usually available.

### What is the right data format for audio and meta data?

First of all: This project specialises in sound records. Recordings of all other data are useful and often important. But we interpret sound data as primary data and all other recorded data as secondary "metadata". There is of course no reason to reduce the content of this metadata to simple attributes if this data is not specifically recorded to be truly only metadata in the sense of attributes. However, this means that we concentrate on preparing our interfaces in such a way that they are particularly well suited for use with opensoundscape:

> **Opensoundscape is the leading Python project for processing environmental audio data.**

### Images as meta or main data

With regard to the question of metadata, image data plays a special role, however, since it can also be available as mass data and, in the context of audio data, it must also undergo similarly complex recognition algorithms. For this reason, it is planned at a later point in time to also organise image data within this framework in a comparable way and to organise it together with audio data in a common database.

## Databases for use with fine-sampled recording data

> **The core question over all is, how we should save our data to be optimal usable?**

Many people start by saving the recorded data in folder structures on their hard drive in the use cases considered here. The data is therefore stored in a tree-like structure. Alternatively, you could also store all the data in a single level and enter all the context information about how this data is related in tables. You can certainly do that. And supporters of SQL databases will do just that and possibly even enter the source data itself in special tables whose columns are defined as a so-called blob data type, where the raw data is copied into. These so-called relational databases are not a bad choice for pure machine data processing in many cases, since they usually have numerous search and filter methods implemented. However, the ‘relations’ of the data become difficult to understand even with simple relationships without the aid of detailed documentation. You can also create groupings and hierarchies here, but these are not visible in the tables at first glance.

The folder structure mentioned above is quite different. The way the data is organised is immediately apparent. The nature of the relationship between the items can often be inferred from the names of the folders. Metadata can also be easily classified as separate files in such a structure. And if the data is not too complexly interwoven and the file and folder names are meaningful, such a structure can be self-explanatory. If you also insert a few README files in the appropriate places to describe the final context and details of the data, you actually have a self-explanatory database with such a folder structure.

In principle, this is sufficient. And that is why it is often practised this way. If the data is also to be archived in a coherent manner, then it is compressed into a zip file (or another common format) and the content can be passed on in one piece. Doing it this way is perfectly legitimate and usually much more user-friendly than a solution with table-based databases.

> **So we won't deviate from a tree-like data structure here.**

However, the folder solution has two major disadvantages:

1. The file system management of the operating system used is misused as a database. This is not what it was actually designed for.
2. Such use, without standards or an API, represents a proprietary solution in which no access methods are defined to match the content of the stored data.

The two issues are unrelated. In principle, it would be sufficient to standardise point 2 with a suitable framework. However, point 1 has a significant disadvantage: when dealing with large amounts of data, distributed across many individual files, the operating system's file system works relatively inefficiently. The data set transferred via compression as a whole unit must first be unzipped to enable direct access to the individual files. And the mechanisms in the operating system, in particular caching, are not necessarily ideal for use as a database system.

This problem was identified many years ago, particularly in scientific applications, and the U.S. [National Center for Supercomputing Applications](https://en.wikipedia.org/wiki/National_Center_for_Supercomputing_Applications) (NSCA) made a name for itself many years ago by developing a so-called hierarchical data format (HDF) and a high-performance API to go with it. The establishment of the HDF Group as a non-profit company in 2006 professionalised this basis and led to the version HDF5 that is in use today. Professionalised means that there is

* hardly a system with a better performance (based on the type of data that is typically stored in this system)
* the library is available for an exceptionally large number of programming languages

What type of data is the system optimised for?

* records and metadata can be organised hierarchically, enabling a first level of self-explanation.
* mass data in the form of tabular data, one-dimensional data blobs or arbitrary dimensional arrays, stored in the structure as data records
* assignment of any amount of metadata to the tree-like structures and to the mass data
* relationships between records can ultimately be defined based on structure and metadata
* descriptions can also be attached to any object in the form of metadata

Another optimisation is that access to the data can be indexed in data sets, so that it is not necessary to load a data set completely into the main memory in order to work with it. Furthermore, file access is optimised to such an extent that it is clearly superior in terms of performance to the above-described variant of storing the data in individual files in a file folder structure.

However, HDF has a significant disadvantage compared to relational databases: it is not designed to process write operations from parallel processes! It is hardly useful as a database for reading AND writing data with high performance. The design of HDF essentially meets the requirements of our application for storing extensive source data and metadata. Using the same HDF5 database file to store the results of the recognition process is possible, but not performance-optimised. Unlike SQL databases, HDF5 does not have an engine that receives and cleverly sorts data queries and write requests. HDF is designed to combine (scientific) mass and source data with all its meta information and to standardise access to it.

So in the end, it is designed to store our wildlife data together with all meta information in a structured and ‘easy to read’ way. If the analysis of the data from the HDF5 file produces relatively little data, or data that is well compressed in terms of content, it is of course no problem to store these results in the same HDF5 file. However, we are only considering the case of summarising the source data well structured and with metadata in an HDF5 file. With the following objectives:

* data that belongs together is also stored together in a file
* the fact that the data belongs together is self-evident from the structure and any ‘descriptive’ metadata that may have been added.
* chunkwise access, even across data that was originally recorded in separate files
* metadata describes the data (e.g. which physical unit is to be used or which conversion into a physical unit is necessary; e.g. with which device combination and at which location the data was generated; how the individual data series can be historically aligned, etc.)
* the associated framework (developed and published here) provides standardisation of the data

The goals show that reusability with the help of the HDF5 format and the framework published here is only possible if the framework itself is accepted and used as a de facto standard by many working groups. Without this effect, the good self-description of the data in the HDF5 files remains. However, each new data set also requires the development of new access methods.

> **The main purpose of this project is therefore to establish the access methods as an API as a quasi-standard.**

> **Accordingly, the project is open to contributors for continuous development and improvement.**

### Explore any HDF5 files

There are a number of tools for opening and exploring any HDF5 file. Generally speaking, there is no problem in recognising the structure and details of data sets and metadata in order to create suitable evaluation scripts. That said, exploring a previously unknown HDF5 file and writing suitable evaluation scripts from it is a straightforward process with a low barrier to entry.

# Module Documentation

[Development process is in progress. Documentation will follow later.]

# Project State

Development for a first version with a reasonable range of functions in progress. Image data are not yet considered.
