Sample Processing
=================

1. Joining paired-ends
----------------------

By assembling or joining each pair of reads we obtain larger sequences, 250 bp with a 50 bp splice on average. The **fastq-join software** implemented in QIIME is used for this task.


    ``join_paired_ends.py -f $PWD/lane1_NoIndex_L001_R1_001.fastq -r $PWD/lane1_NoIndex_L001_R3_001.fastq -b $PWD/fastq-join_joined/fastqjoin.join_barcodes.fastq -o $PWD/fastq-join_joined``

The forward reads ``lane1_NoIndex_L001_R1_001.fastq`` are assembled with the reverse reads ``lane1_NoIndex_L001_R3_001.fastq`` and a new file of the barcodes of the resulting sequences is generated ``fastq-join_barcodes.fastq``

In this way we obtain the ``fastqjoin.join.fastq`` file with the reads already joined, with an average size of 250 bp each. These sequences will be the basis for subsequent analyzes, but first we must identify which samples they correspond to. For this we use the new barcodes file ``fastqjoin.join_barcodes.fastq`` and we do what is known as demultiplexing.

Mapping File
------------
The `mapping file <https://docs.google.com/spreadsheets/d/12RWPWSIBbB9NrSCJvN0kDUEyX7CZdCuwbMW32ZskMrM/edit?usp=sharing>`_ is a simple, tab-separated text file that includes the names of the samples and their metadata and categories for the analysis.

2. Demultiplexing
-----------------

It consists of identifying (adding the name) to which sample each sequence belongs using the index or barcode that we use to build the libraries. The sequence data in ``fastqjoin.join.fastq`` is processed using the `MexBC-9c.csv <https://docs.google.com/spreadsheets/d/12RWPWSIBbB9NrSCJvN0kDUEyX7CZdCuwbMW32ZskMrM>`_ metadata file where the barcodes are located.

The data in ``fastqjoin.join.fastq`` is processed using the `MexBC-9c.csv <https://docs.google.com/spreadsheets/d/12RWPWSIBbB9NrSCJvN0kDUEyX7CZdCuwbMW32ZskMrM>`_ metadata file where the barcodes are located.


    To later use MICCA, we follow point 2.2 that is detailed in :ref:`MICCA pipeline <miccapip>`.

Note the ``--rev_comp_mapping_barcodes`` option to consider cases where the index is reverse complementary. In the same way, the ``--store_demultiplexed_fastq`` option if we want to examine the new fastq file later with FastQC.

    ``split_libraries_fastq.py -i fastqjoin.join.fastq -o $PWD/sl_out_q20 -b fastqjoin.join_barcodes.fastq -m ../MexBC-9c.csv --rev_comp_mapping_barcodes --max_barcode_errors 4 --store_qual_scores --store_demultiplexed_fastq -q 19``

The ``--max_barcode_error 4`` option allows a maximum of 4 barcode errors. This is acceptable as our barcodes are large, 12 bp.

The ``--store_qual_scores`` option allows us to obtain the seqs.qual file

The ``-q 19`` option defines a minimum quality of Q20 (Quality filter at Phred> = Q20).

*We verify the obtained file using also the mapping file*

    ``validate_demultiplexed_fasta.py -i $PWD/sl_out_q20/seqs.fna -m ../MexBC-9c.csv``

