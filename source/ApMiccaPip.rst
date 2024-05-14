.. _apmiccapip:

Appendix MICCA pipeline
=======================

Merge multiple samples in MICCA (merge)
---------------------------------------

You have multiple fastq files (one for each sample) which will be merged into a single file.

    ``micca merge -i splitted_bySample_fastq/*.fastq -o merged_allSamples.fastq``

We evaluate the data to define the values for filtering the sequences, this implies identifying the percentage of error that we will allow and the ideal size of the sequences to do the filtering.

    ``micca filterstats -i merged_allSamples.fastq -o filterstats``

We review the graph ``filterstats_plot.png`` and the file ``filterstats_minlen.txt``. We observe that we can choose a maximum error rate of 0.5% and a length of 252 bp, this gives us that 97.840% of the sequences pass the filter. This is a fairly acceptable percentage of data with which we can continue the analysis.

    
    +-----+--------+--------+--------+--------+--------+-------+
    | L   |  0.25  |  0.5   |  0.75  |  1.0   |  1.25  |  1.5  |
    +=====+========+========+========+========+========+=======+
    | 249 | 91.766 | 97.957 | 98.714 | 98.766 | 98.773 | 98.775|
    +-----+--------+--------+--------+--------+--------+-------+
    | 250 | 91.761 | 97.949 | 98.706 | 98.758 | 98.764 | 98.766|
    +-----+--------+--------+--------+--------+--------+-------+
    | 251 | 91.755 | 97.942 | 98.698 | 98.750 | 98.757 | 98.759|
    +-----+--------+--------+--------+--------+--------+-------+
    | 252 | 91.661 | 97.840 | 98.595 | 98.647 | 98.654 | 98.656|
    +-----+--------+--------+--------+--------+--------+-------+
    | 253 | 68.182 | 72.715 | 73.280 | 73.313 | 73.315 | 73.316|
    +-----+--------+--------+--------+--------+--------+-------+
    | 254 | 0.484  | 0.521  | 0.525  | 0.526  | 0.526  | 0.527 |
    +-----+--------+--------+--------+--------+--------+-------+
    | 255 | 0.015  | 0.018  | 0.019  | 0.019  | 0.020  | 0.020 |
    +-----+--------+--------+--------+--------+--------+-------+
    | 256 | 0.012  | 0.014  | 0.015  | 0.016  | 0.016  | 0.016 |
    +-----+--------+--------+--------+--------+--------+-------+

Apply the filter

    ``micca filter -i merged_allSamples.fastq -o merged_allSamples_filtered.fasta -e 0.5 -m 252``

We obtain all the samples with their sequences cut to 252 bp, which add up to 2507261 sequences (total reads) and in fasta format.

We build OTUs at 97% identity (greedy de novo)

    ``micca otu -i merged_allSamples_filtered.fasta -o denovo_greedy_otus -d 0.97 -c -t 64``

The ``-c`` option is to search for qimeras and ``-t`` to use 64 threads (processor cores, adjust depending on the computer).