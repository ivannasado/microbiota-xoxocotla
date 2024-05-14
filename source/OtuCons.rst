OTU construction
================

.. _otucons:

3. OTU construction
-------------------

This implies grouping sequences by similarity, defining clusters and searching the ribosomal databases to identify OTUs, both by reference (finding similarities with already known sequences) and by *de novo* methods to identify new OTUs.

We make a configuration file ``uc_fast_params.txt``.

    ``echo 'pick_otus:enable_rev_strand_match True' &gt; uc_fast_params.txt``

We execute the *open_reference* algorithm with 97% identity at the species level. Option ``-a -O 8`` is to use 8 CPUs (modify according to the capacity of our computer).

4. Sequences per sample (sequencing depth)
------------------------------------------

We can obtain the number of sequences per sample from the BIOM table that was generated in the OTUs constuction step. This allow us to know which samples need to be eliminated from the analysis because they have very few sequences. The cutoff (minimum) value required for the diversity analysis to be valid is defined according to the depth of sequencing, but it is common to work with 5,000 or more sequences per sample. The more sequences per sample are required, the more samples will be excluded from the analysis. For example, in our data, if we decide to discard samples below 6183 sequences, we leave out of the analysis two samples out of a total of 90. The rarefaction curves are constructed from random resamples up to this minimum value that we have defined previously.

We construct a table ``biom.summarize-table.txt`` with the statistics of the number of sequences per sample.

    ``biom summarize-table -i open_ref_otus_97/otu_table_mc2_w_tax_no_pynast_failures.biom &gt; biom.summarize-table.txt``

When examining the file we can arbitrarily choose the value of 6183.

    ``less biom.summary.table``

    Num samples: 90
    
    Num observations: 17258
    
    Total count: 2535641
   
    Table density (fraction of non-zero values): 0.046

    Counts/sample summary:

    Min: 50.0

    Max: 96668.0
    
    Median: 26614.000
    
    Mean: 28173.789
    
    Std. dev.: 13211.364
    
    Sample Metadata Categories: None provided
    
    Observation Metadata Categories: taxonomy

    Counts/sample detail:
    
    E032: 50.0
    
    E019: 5263.0
    
    HE074.06M: 6183.0
    
    HE019.09M: 7290.0
    
    HE036.15M: 8246.0
    
    HE070.06M: 11458.0

5. Filtering OTUs with very low number of sequences
---------------------------------------------------

OTUs are eliminated using a 0.00005 filter equivalent to 0.005% of our total data. According to previous studies (see [Bokulich et al., 2013]) this step is suggested to avoid distortions in the analysis of biodiversity due to an over-representation of OTUs that are possibly spurious or that are an artifact of the methodology. In this case, we only consider OTUs that are supported approximately with at least 300 sequences (considering a total data of 6,000,000 sequences).

The BIOM table of OTUs is filtered, eliminating all those OTUs supported by less than 0.005% of the total sequences.

    ``filter_otus_from_otu_table.py -i $PWD/open_ref_otus_97/otu_table_mc2_w_tax_no_pynast_failures.biom -o $PWD/open_ref_otus_97/otu_table_filtered-0.00005.biom --min_count_fraction 0.00005``

We obtain the statistics of the new BIOM table
    
    ``biom summarize-table -i open_ref_otus_97/otu_table_filtered-0.00005.biom &gt; biom.summarize-table_filtered-0.00005.txt``

We observed that despite the OTU filter, the number of sequences per sample did not change substantially. We can leave a minimum
of 5521 sequences per sample, eliminating only two samples (E032 and E019) from the analysis.

    
    Num samples: 90
    
    Num observations: 723
    
    Total count: 2376391
    
    Table density (fraction of non-zero values): 0.304
    
    Counts/sample summary:
    
    Min: 45.0
    
    Max: 92847.0
    
    Median: 24687.000
    
    Mean: 26404.344
    
    Std. dev.: 12617.321
    
    Sample Metadata Categories: None provided
    
    Observation Metadata Categories: taxonomy
    
    Counts/sample detail:
    
    E032: 45.0
    
    E019: 4689.0
    
    HE074.06M: 5521.0
    
    HE019.09M: 6978.0
    
    HE036.15M: 7700.0
    
    HE070.06M: 10700.0
    
    HE006.12M: 10856.0

6. Diversity analysis
---------------------

We include in the analysis the categories **SampleType**, **AgeYears**, **BirthType** (see mapping file MexBC-9c.csv) and we use the BIOM table without the filter. Option ``-a -O 14`` means to use 14 CPUs (it is determined by the number of CPU cores in our computer).

    ``core_diversity_analyses.py --recover_from_failure -c "SampleType,AgeYears,BirthType" -i $PWD/open_ref_otus_97/otu_table_mc2_w_tax_no_pynast_failures.biom -m $PWD/../MexBC-2c.csv -t $PWD/open_ref_otus_97/rep_set.tre -e 36507 -o $PWD/diversity_analysis/open_ref -a -O 14``

The same but using the BIOM table with the 0.005% filter.

    ``core_diversity_analyses.py --recover_from_failure -c "SampleType,Family,AgeYears,Intervals,BirthType,OtherParasites,BMI_category,Nutrition" -i $PWD/open_ref_otus_97/otu_table_filtered-0.00005.biom -m $PWD/../../MexBC-9c.csv -t $PWD/open_ref_otus_97/rep_set.tre -e 5521 -o $PWD/diversity_analysis_filtered/open_ref -a -O 32``

The results will be available in the directory diversity_analysis ``/ open_ref /`` and those where we apply the filter will be in ``diversity_analysis_filtered / open_ref /``

We use the web browser to see the results.

    ``firefox diversity_analysis_filtered/open_ref/index.html``
