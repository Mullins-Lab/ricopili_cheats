# RICOPILI troubleshooting
RICOPILI errors we've run into and how to fix them. This is for the version of RICOPLI installed on Snellius according to [these instructions]().

# Preimputation (QC) module

1. error: "->system call failed: 1 at /home/pgca1sui/apr2023_ricopili/ricopili/rp_bin/rep_qc2_14 line 482." 
   problem: there are snp IDs that contain "#" and R is reading this as a comment and can't properly read the "nofi_yourdata.lmiss" file.  
   solution: make a couple of changes to the rep_qc2_14 script 
   line 3370:
   ```
   my $R_hist_templ='read.table("LMISS", header=T, comment.char="")-> snp_miss
   ```
   line 3788:
    ```
   my $R_in='read.table("INNAME", header=T, comment.char="")-> all'."\n";
   ```
3. x
4. x

# PCA module

# Imputation module

1. error: all subdirectories (except for qc1) of the dasuqc* folder are empty
   problem: this version of ricopili includes a cleaning that automatically deletes intermediate files created during imputation 
   solution: use the --noclean flag with the impute_dirsub command.

# Postimputation module

