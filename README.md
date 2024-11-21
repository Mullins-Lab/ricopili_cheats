# RICOPILI troubleshooting
RICOPILI errors we've run into and how to fix them. This is for the version of RICOPLI installed on Snellius according to [these instructions](https://github.com/Mullins-Lab/snellius/blob/main/rico_install2.md).

# Preimputation (QC) module

1. error: "->system call failed: 1 at /home/pgca1sui/apr2023_ricopili/ricopili/rp_bin/rep_qc2_14 line 482." 

   problem: there are snp IDs that contain "#" and R is reading this as a comment and can't properly read the "nofi_yourdata.lmiss" file.  

   solution: make a couple of changes to the rep_qc2_14 script in your rp_bin

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

1. error: Argument "ALL" isn't numeric in addition (+) at /home/user/apr2023_ricopili/ricopili/rp_bin/danerdir_6 line 772, <FILE> line 1.

   problem: ricopili was having issues reading the PCs using the --coco ALL argument, not sure if this was a ricopili problem or maybe something with the formatting of the covariates file? but either way the below solution will work.

   solution: instead of using `--coco ALL` need to use specific PCs like `--coco 1,2,3,4,5,6`

2. error: the `--onlymeta` flag appears to go past the meta-analysis and outputs figures, etc.

   problem: this occurs when using the `--popname` flag in conjunction with the onlymeta option. it's not necessarily a problem, because you still get all the output you need, but you will also get extra output you didn't want. no "solution" provided since it technically runs everything okay, but just making a note of it here.

3. error: pdfjam ERROR: LaTeX package everyshi.sty not installed (see the pdfpages manual)

   problem: when snellius was updated, some latex package dependencies were lost

   solution: run the follow before launching the postimp module: `module add 2022; module add texlive/20230313-GCC-11.3.0`

4. error: areator Error: No variants remaining after --extract.

   problem: in my case, the areator step wasn't working for two cohorts because the columns were not in the correct ricopili order (chr and snp were switched)

   solution: first culprits will be cohorts that weren't processed with ricopili and therefore may have misordered columns. fix the column order for those summary statistics and then everything should run okay.

5. error: llegal division by zero at /home/pgca1sui/apr2023_ricopili/ricopili/rp_bin/metaber7 line 1095, <META> line X.

   problem: when using --nofilter flag alone, the INFO score filter was still being applied after gathering SNPs, so sometimes ending up with Neff=0

   solution: combine all the filter flags to make sure absolutely no filtering occurs like `--info_th 0.0 --nofilter --no_neff_filter`
