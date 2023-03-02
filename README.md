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

Data was generated in 2017-2018. PacBio reads were the basis of the assembly, with HiSeqX dat used to refine the assmebly and increase quality.

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

Conversion was done using the command line:

EMBLmyGFF3 -i GBAR -p [ENA project] -r 1 -s 'Geodia barretti' -t linear -m 'genomic DNA' geodia_barretti_ENAcompliant.gff genome.fa -o [output file].embl

