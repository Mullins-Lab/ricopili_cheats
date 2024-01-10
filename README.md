# RICOPILI CHEATS
RICOPILI errors we've run into and how to fix them 

# PREIMP_DIR

1. error: "->system call failed: 1 at /home/pgca1sui/apr2023_ricopili/ricopili/rp_bin/rep_qc2_14 line 482."
   problem: there are snp IDs that contain "#" and R is reading this as a comment and can't properly read the "nofi_yourdata.lmiss" file. 
   solution: in the "qc/tmp_report*" directory, edit the "lmiss_hist_Rin" script's first line like:
   ```
   read.table("nofi_yourdata.lmiss", header=T, comment.char = "")-> snp_miss
   ```
3. x
4. x

# PCAER

# IMPUTE_DIRSUB

# POSTIMP_NAVI

