# msa2cscore.py
## Instructions
### Arguments
The code is executed using python and takes 3 arguments:
    1: path to the fasta alignment
    2: path to the protein file (with contacts and pscores)
    3: name (path) of the output file
**Example: python3 msa2cscore.py path/to/FAMILY.fa path/to/PROTEIN.contactbench OUTPUT.file**

### Type of files
#### MULTIPLE SEQUENCE ALIGNMENT FILE
The MSA should be written in fasta format:

***>namesequence1***

**fastafastafastafasta...**

***>namesequence2***

**fastafastafastafasta...**

**...**

Here, the _namesequence_ are the identifiers of the sequence. May or may not be the same as the contact file ones. If they are not the same, the code will need an extra file that maps each identifier of the MSA with the one of the contact files (see "Identifiers file" lower).

#### CONTACTS FILE
The file containing the contacts should follow the following format:

__left    right   distance    pscore      *TRUE/FALSE__

_*(TRUE/FALSE is optional. See in Procedure)_

It is mandatory that all the files of the family (sequences of the msa) containing the contacts ends with *".contactbench"*.

**Example: 1ATTA-1.contactbench**

         **10  50  40  0.987   TRUE**

Both the main _.contactbench_ file and the rest of the files corresponding to the same family should be located in the same directory. This directory must also be called as the FAMILY they belong.

#### IDENTIFIERS FILE (OPTIONAL)
This file is only needed just if the identifiers of the multiple sequence alignment are **not the same as the contact files** (for example, having the pfam identifiers in the alignment and the pdb identifier in the contact file). It should be given to the program as the fourth argument, as in the following example:

**Example: python3 msa2cscore.py path/to/FAMILY.fa path/to/PROTEIN.contactbench OUTPUT.file path/to/FAMILY.idenfifiers**

It must contain in each line first the identifier of the MSA followed by a second element that separates them, and then the identifier of the contact file.

**Example: FAMILY_ref.template_list**

         **PFAM_id separator PDB_id**

(The code reads the first column and the third one, that's why the separator is needed as a second column.)

#### OUTPUT FILE
The output file can be named as the user wish, but is important to note that, if the name is the one of a file that already exists, the program will delete it and rewrite it.

By default, the output file will be located in the same directory as the program is, unless the user adds extra path starting from the current directory.

**Example 1: output.file**

**Example 2: path/to/output.file**

_("path" is a directory inside the directory where msa2cscore.py is located)_

### Procedure
Once the arguments are introduced and the program starts, it will first ask for a threshold for the pscore. This threshold specifies to the program that it should only take into account the contacts with equal or higher pscore. This value must be between 0-1.

It also asks for the threshold of the cscore, so that in the final output, only contacts with a cscore higher than the threshold will be written. Must also be between 0-1

The program asks if weights should be considered.
If yes ('y'), the program will calculate the Voronoid weights using the multiple sequence alignment as input.
In order to do this, the user must have installed SQUID, obtainable in the following link: http://eddylab.org/software.html and copied the binary file "weight" to a directory included in the PATH environmental variable.

The program then asks if the alignment identifiers are the same as the same as the sequence contact files. If so, 'y' must be pressed and 'n' if not. If they are not the same and 'n' was pressed, the program will look for the "_ref.template_list" explained in the IDENTIFIER FILE section.

The program will start running. If no errors are raised, the output file will be written following the following format:

**name  left    right   pscore  cscore  correct**    

__Name:__ identifier of the sequence

__Left:__ left residue

__Right:__ right residue

__pscore:__ predicted score of the contact

__cscore:__ conservation score of the contact

__correct:__ TRUE (1) or FALSE (0) contact (if not taken into account, won't appear)

The file includes a header with the previous line to make it clear in the file itself.
