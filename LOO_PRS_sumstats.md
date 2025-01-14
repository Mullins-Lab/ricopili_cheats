The [ricopili LOO GWAS module](https://docs.google.com/document/d/10UKUfX4wT9rP5zswH5SDR2Y1O2-wJVUw9mn4tSdaS-I/edit?tab=t.0) only works when using only indiviudal level data all run with RICOPILI. Here I outline instructions for how to run LOO GWAS when summary stats were used to run the meta-analysis (assuming you followed this method to run postimp_navi: https://docs.google.com/document/d/1o4bN_uLK4IEItXCSdeQkXfZwEpLuSCWlveJevRogi08/edit?tab=t.0#heading=h.4008addvumol).

## Run this code to set up the loo results directory:

```
## go to the directory that you ran the meta-analysis in
cd /gpfs/work5/0/pgcdac/DWFV2CJb8Piv_0116_pgc_data/pgcdrc/sui/stage2_ready/wave1/si/afr/all

## set a bash variable with the name of your original ricopili meta-analysis output (this should be what was supplied to the --out and --addout flags of postimp_navi)
identifier=pgcsui_w1_si_afr_all7_20241106

## now make the results directory
results_name=results_${identifier}
mkdir ${results_name}
cd ${results_name}

## now copy the dataset_dir file you used in the --results flag of the postimp_navi command and rename it
cp ../dataset_dir ./${results_name}

## make symbolic links to the sum stats files in dataset_dir
while read line; do
  ln -s ../"$line"
done < ${results_name}

## make symbolic link to the chunked files and reference
ln -s ../resdaner
ln -s ../reference_info
```

## Then you can create a script that will run all the LOO GWAS using this code: 

```
## make list of sumstats
IFS=$'\n' read -d '' -r -a sumstats < ${results_name}

## make command file
cmd_file="start_loo_weights.cmd"
echo -n > "$cmd_file" || { echo "Error creating $cmd_file"; exit 1; }

## loop over sumstats
for loof in "${sumstats[@]}"; do
    loof_cells=(${loof//_/ })  ## split on underscores
    loof_sub="${loof_cells[0]}"  ## first component of the filename

    ### Process results files ###
    results_file="results_${identifier}.loo.no.${loof_sub}"
    > "$results_file" || { echo "Error creating $results_file"; exit 1; }

    for loo in "${sumstats[@]}"; do
        loo_cells=(${loo//_/ })  ## split on underscores
        loo_sub="${loo_cells[0]}"  ## first component of the filename
        if [[ "$loof_sub" != "$loo_sub" ]]; then
            echo "$loo" >> "$results_file"
        fi
    done

    ### Generate weights command ###
    echo "postimp_navi --out ${identifier}.loo.no.${loof_sub} --result $results_file --nolahunt --noldsc --gwclump" >> "$cmd_file"

done

```
