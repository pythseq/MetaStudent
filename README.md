# METASTUDENT 
## Predictor of gene ontology terms from protein sequence

Metastudent predicts Gene Ontology (GO) terms from the Molecular
Function Ontology (MFO) and Biological Process Ontology (BPO) for input
protein sequences by homology-based inference from already annotated
proteins.

**Development Year:**  2012

**Authors:** Tobias Hamp <hampt@rostlab.org>

**Publications:** Hamp, T., Kassner, R., Seemayer, S., Vicedo, E., Schaefer, C., Achten, D., ... & Rost, B. (2013),
"Homology-based inference sets the bar high for protein function prediction", BMC Bioinformatics, 14(Suppl 3), S7.

**Manpage:** http://manpages.ubuntu.com/manpages/saucy/man1/metastudent.1.html#contenttoc8

**Documentation:** https://rostlab.org/owiki/index.php/Metastudent

**Elixir:** https://bio.tools/tool/tum.de/MetaStudent/1

## Installation
Metastudent can easily be installed using apt-get command on any debian based system
```
sudo apt-get install metastudent

```
All the Metastudent related packages will also be installed. Type 'Y' on the console query to accept installation of all packages.

Metastudent-Data is required for the execution of the program.
Metastudent data will also be installed while installing metastudent package. If the data is not there, the data can be found on the following links.
Please find the data at the following link:

```
https://www.dropbox.com/sh/3hm0w3jom6hwr46/AABIATIewd_byccHGUK89tQxa?dl=0
```
Add the metastudent-data path accordingly in the config file.

Make sure that the *blastpgp* program is available. If not downloaded automatically ( check /usr/bin/blastpgp program's existance), you can download *blastpgp* from package *blast-2.2.26* for the corresponding platform from the following FTP: 
```
ftp://ftp.ncbi.nlm.nih.gov/blast/executables/release/LATEST/
```
For Linux x64 please follow the following commands. With these commands *blastpgp* binaries are installed and added tho the PATH environment variable:
```
wget ftp://ftp.ncbi.nlm.nih.gov/blast/executables/release/LATEST/blast-2.2.26-x64-linux.tar.gz
tar xf blast-2.2.26-x64-linux.tar.gz
echo -e "[NCBI]\nData=$(pwd)/blast-2.2.26/data/" > ~/.ncbirc
export PATH=$PATH:$(pwd)/blast-2.2.26/bin
```
NOTE: Following issues have been noticed while running of metastudent. These steps may be necessary for execution
```
Confirm that the metastudentpkg folder in the python dist-package folder ( e.g. /usr/lib/python2.7/dist-packages/) contains commons.py file. If not, then copy commons.py from the git repository to the folder.
```

## Configuration
Metastudent can be configured with a configuration file. The default Metastudent configuration are present in:
```
<package_data_dir>/metastudentrc.default
```
The following parameters must be set before running the program
The paths and various parameters can be changed by changes in this file.

If the user wants to use its own configuration file, then --config flag can be used
Note:
```
Confirm the parameter BLAST_SRC_CCO is present in config file being used. It was not present in the default configuration file. If BLAST_SRC_CCO is not present, then CCO prediction will not be processed and an EXCEPTION will be raised.
Default: BLAST_SRC_CCO=goasp
```

## Running Metastudent
Metastudent can be run by the following command after installation and configuration:
```
metastudent -i FASTA_FILE -o RESULT_FILE_PREFIX [--debug] [--keep-temp]
[--silent] [--output-blast] [--blast-only] [--all-predictions]
[--ontologies=MFO or BPO or MFO,BPO]
[--blast-kickstart-databases=BLAST_RESULT_FILE(S)] [--temp-dir=DIR]
[--config=CONFIG_FILE] 
```
*Please make sure your fasta file contains at most 500 sequences.*

### OPTIONS

       -i FASTA_FILE
           The input fasta file. Please try to remove any special formattings
           (e.g. whitespaces) in the sequences before using them as input. Due
           to high memory usage, make sure your fasta file contains at most
           500 sequences.

       -o RESULT_FILE_PREFIX
           The file name prefix of the output files. GO terms are organized in
           ontologies. Metatstudent treats each ontology differently and
           outputs one result file for each. For example, if
           <RESULT_FILE>=./myresult and MFO (Molecular Function Ontology) and
           BPO (Biological Process Ontology) ontologies are selected (see
           option --ontologies), then metastudent creates two output files:
           ./myresult.MFO.txt and ./myresult.BPO.txt.

       --debug
           Print extra debugging messages.

       --keep-temp
           Whether to keep the temp directories after metastudent has finished
           (they can be useful when errors occur or in combination with
           --blast-kickstart-databases).

       --silent
           No progress messages (stdout), only errors (stderr).

       --output-blast
           Whether to output the result of the BLAST runs. Useful in
           combination with --blast-kickstart-databases. Output file name
           format is RESULT_FILE_PREFIX.<BLAST_OPTIONS>.blast.

       --blast-only
           Whether to only output the result of the BLAST runs, and nothing
           else. See options --output-blast and --blast-kickstart-databases.

       --all-predictions
           Whether to output the prediction results of the individual
           predictors. File name format of the output file is
           <RESULT_FILE_PREFIX>.<ONTOLOGY>.<METHOD>.txt.

       --ontologies=MFO or BPO or MFO,BPO
           A comma separated list of ontologies to create predictions for.
           Default is MFO,BPO. If used in combination with
           --blast-kickstart-databases, the number and order of the ontologies
           must correspond to the kickstart files.

       --blast-kickstart-databases=<BLAST_RESULT_FILES>
           Since running BLAST is usually the part that takes the longest in
           metastudent, this option allows you to re-use the output of a
           previous run. This is useful to test, for example, different
           parameters or when you have lost a prediction. The number of
           kickstart files must correspond to the number of ontologies (see
           option --ontologies). Separate the file paths with commas. For
           example:
           --blast-kickstart-databases=<RESULT_FILE_MFO>,<RESULT_FILE_BPO>
           (kickstart for both ontologies) or
           --blast-kickstart-databases=,<RESULT_FILE_BPO> (only kickstart BPO;
           note the comma).

       --temp-dir=DIR
           The parent temp directory to use instead of the one specified with
           tmpDir in the metastudent configuration file.

       --config=FILE
           The path to a custom metastudent configuration file; overrides all
           settings of the configuration files found in the FILES section of
           this man page.

### FILES

       <package_data_dir>/metastudentrc.default
           The metastudent configuration file.

       <sysconfdir>/metastudentrc
           The metastudent configuration file, overrides
           <package_data_dir>/metastudentrc.default.

       <homedir>/.metastudentrc
           The metastudent configuration file, overrides
           <sysconfdir>/metastudentrc.

### EXAMPLES

       The example test.fasta file can be found in <package_doc_dir>/examples
       (usually /usr/share/doc/metastudent/examples).

       Predict the GO terms for the sequences in test.fasta for both the MFO
       and the BPO ontology:
            metastudent -i test.fasta -o test.result

       Create the BLAST output to predict the MFO terms for sequences in
       test.fasta (not the actual predictions, yet; see next example).
            metastudent -i test.fasta -o test.result --blast-only --output-blast --ontologies=MFO

       Predict the MFO and BPO terms for sequences in test.fasta with a
       precomputed MFO BLAST output (see previous example; note the comma at
       the end).
            metastudent -i test.fasta -o test.result --ontologies=MFO,BPO 
            --blast-kickstart-databases=test.result_eval0.001_iters3_srcexp.mfo.blast,

### OUTPUT FORMAT

       For each selected ontology (see --ontologies), one output file is produced (see -o). 
       Each line in each file associates a protein with a GO term and a reliability for
       the association (0.0 to 1.0). The following format is used: 
       <PROTEIN ID><TAB><GO_TERM><TAB><RELIABILITY>


## HOWTO generate the distributable tar archive
```
$ setup.py sdist
```
## Method Description

To be UPDATED

* Description (ML ? )
* Training / Test Data
* ...

## Evaluation

TO BE UPDATED

Perhaps:

* Performance measures used (F1 ?, Accuracy ?, ROC Curve ?, ...)
* Comparison with other tools
* ...
