import_scans
============

scripts to import MRI scans from neurospin's  acquisition database

import_subject_from_trio
------------------------

@@import_subjects_from_trio@@ allows one to import MRI scans from @@/neurospin/acquistion@@ and convert them into the 4D NIfTI format
suitable for FSL or SPM8.

Usage:
[@
   import_subjects_from_trio [-a ANAT_DIR -f FMRI_DIR] < list_subjects.txt
@]


-a ANAT_DIR (optional): specifies the subdir for the anat*.nii file [by default: t1mri/acquisition1)

-f FMRI_DIR (optional): specifies the subdir for all other *.nii files [by default: fMRI/acquisition1)

The textual input (e.g. @@list_subjects.txt@@) describes the scans to download. Here is an example:

[@
 subj01  20110404 fp110067 02 anat 05 norm1 07 jabb1 09 norm2 
 subj02  20110404 kl198789 02 anat 05 norm1 07 jabb1 09 norm2 
  ...
@]

There is one line per participant: 

- The first item of each line is the name of the directory where this participant's data will be saved.
- The second item is the date of acquisition in the format YYYYMMDD,
- The third item is the participant's NIP identifier
- the remaining of the line is a series of pairs 'series_number' and 'target_name'. Each series is identified by a *two-digits number* (i.e. *01* rather than *1*), the "target name" will be added as a prefix to the file name for
this series.


Notes:
* to download the scripts: 
[@ 
git clone https://github.com/chrplr/import_scans.git
@]
This will create a directory "import_scans". To install the scripts, just copy the files to a directory listed in your PATH variable (e.g. "$HOME/bin"). 

* This script reads from the standard input; therefore you can use 'grep' to extract the relevant lines from the list if you want to include only some participants. Also, lines starting with '#' are ignored, so you can add '#' in front of the subjects that you do not want to download.

* The conversion from Dicom to nifti is handled by [[http://www.mccauslandcenter.sc.edu/mricro/mricron/dcm2nii.html | dcm2nii]].

* this script automatically puts the data into the @@t1mri/acquisition1@@ and @@fMRI/acquisition1@@ subfolders respecting the conventions of the  brainvisa software in use at Neurospin. However, if you prefer to use a different naming convention, the script has two options '-a' and '-f' options to specify the ANAT_DIR and FMRI_DIR respectively:

For example, to specify that the files are to be saved in 'anat' and 'fMRI' subdirs: 
[@
   import_subjects_from_trio -a anat -f fMRI < list_subjects.txt
@]

If a given subject has undergone several independant scanning sessions at different times, you can use -a anat/acquisition2 -f fMRI/acquisition2.



