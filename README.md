# geodia-data-submission-22-12-to-23-03
Description of data submission process of Sponge (Geodia) data in NBIS LTS project to ENA.

# 1. Project Orientation

Contact was made by Bioinformatician in November 2022 to include DM in data submission request by researchers. 

Data types were listed as:

Raw data:
  - Illumina HiSeqX
  - PacBio RSII reads
  - PacBio Sequel reads
     
Assemblies:
  - Annotated nuclear genome
  - Annotated Mitochondrial genome

Data was generated in 2017-2018. PacBio reads were the basis of the assembly, with HiSeqX data used to refine the assmebly and increase quality.

# 2. First scheduled meeting

First meeting between Data Steward (DS) and researchers on 2022-12-02 via Zoom. This was followed up in the week after with communication between Bioinformaticians and researchers on what files to submit, resulting in a list of files for submission consisting of:

  - 1 file of PacBio RSII filtered subreads in .FASTQ format (1.02G zipped)
  - 4 files of PacBio Sequel reads in .BAM format (3.74G + 4.05G + 2.6G + 2.43G unzipped)
  - 2 files of HiSeqX reads in .FASTQ format (37.58G + 42.99G zipped)

  - 1 annotated nuclear assembly in .GFF format prepared for ENA by Bioinfomratician (351M unzipped)
  - 1 annotated mitochondrial assembly in .GFF format prepared for ENA by researchers (8kb unzipped)

# 3. ENA account - Registration of project and study 

Resarchers created account in ENA and registered project and study on 2022-12-05. Provided DS with ENA login information to act as broker.

Data Steward decided to begin submission with the PacBio RSII sequel reads. Single file, small file size.

  - Submission to be done using the ENA webin-cli client due to the various file formats in the projects. Keeping all submissions to a single solution makes trouble shooting easier.
  - Researchers provided DS with access to project folder at UPPMAX, for brokering purposes.
  - DS provided with paths to respective files
> Note: The file tree was rather complex and not easy to navigate. Printed a tree topology to store locally on laptop to facilitate orientation

# 4. Locus tags

To prepare the data for submission proper locus tags had to be decided. After consultation the researchers decided on the abbreviation GBAR for the species under study (Geodia barretti). With the locus tag decided the conversion of .GFF to .EMBL could be made using emblmygff3.

# 5. Preparing for first submission of data incl. creating the manifest file

The PacBio RSII .FASTQ file was downloaded from UPPMAX and verified. After verification the same file was gzipped using the command line with `-k` flag to retain the original file after zipping:

`gzip -k pb_387_filtered_subreads.fastq > pb_387_filtered_subreads.fastq.gz`

A manifest file was first attempted in .JSON format, but the configuration of the file was difficult to figure out, and a change to simple .TXT format was made, naming the file `manifest_RSII.txt`. 

`STUDY PRJEB58046
SAMPLE ERS14471852
NAME pb_387
INSTRUMENT PacBio RS II
INSERT_SIZE 4500
LIBRARY_SOURCE METAGENOMIC
LIBRARY_SELECTION RANDOM
LIBRARY_STRATEGY WGS
FASTQ pb_387_filtered_subreads.fastq.gz`

Information for `INSTRUMENT` was taken from the ENA web help pages. Information for `INSERT SIZE`, `LIBRARY SOURCE`, `LIBRARY SELECTION`, and `LIBRARY STRATEGY` was provided by the researchers. Information for `NAME` was decided by DS in consultation with colleague. ENA website provided very few suggestions on format and what type of information `NAME` should allude to. It was eventually decided to let `NAME` reference the library name.

# 6. First submission round (PacBio RSII reads)

Using the webin-cli client (version 5.2.0) the submission was first validated using the command line:

`java -jar webin-cli-5.2.0.jar -context=reads -manifest=manifest_RSII.txt -userName=[username] -password=[password] -validate`

After validation pass the command line was changed to 

`java -jar webin-cli-5.2.0.jar -context=reads -manifest=manifest_RSII.txt -userName=[username] -password=[password] -submit`

A successful submission was verified by command line output.

# 7. Second submission round (PacBio Sequel reads)

Sinilar to the previous submission of RSII reads (# 5 and # 6) 


Conversion was done using the command line:
`EMBLmyGFF3 -i GBAR -p [ENA project] -r 1 -s 'Geodia barretti' -t linear -m 'genomic DNA' [input ENA gff file].gff [fastq file].fa -o [output file].embl`

The resulting EMBL flat file was 

