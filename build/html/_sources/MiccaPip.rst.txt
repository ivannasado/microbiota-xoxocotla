MICCA pipeline
==============

.. _miccapip:

2.2 To use MICCA pipeline
-------------------------

Export sequences to fastq format
Sample names were changed to accommodate the `MICCA <https://compmetagen.github.io/micca/>`_ pipeline. The new metadata file is `MexBC-9c.csv <https://docs.google.com/spreadsheets/d/12RWPWSIBbB9NrSCJvN0kDUEyX7CZdCuwbMW32ZskMrM>`_. The difference is that the sample names are simplified to one word.

The demultiplexed ``seqs.fastq`` file is generated using QIIME.

    ``split_libraries_fastq.py -i fastqjoin.join.fastq -o $PWD/sl_MexBC-10c_q20 -b fastqjoin.join_barcodes.fastq -m MexBC-10c.csv --rev_comp_mapping_barcodes --max_barcode_errors 4  --store_demultiplexed_fastq -q 19``

2.3 Separate each sample into separated fastq files
---------------------------------------------------

We enter the ``sl_MexBC-10c_q20`` directory and from the demultiplexed sequences in seqs.fastq we generate a fastq file for each of the 90 samples.

We use QIIME to generate multiple fastq files, one per sample.

    ``split_sequence_file_on_sample_ids.py -i seqs.fastq --file_type fastq -o splitted_bySample_fastq/```

We see the files (samples) already separated

    ``ls splitted_bySample_fastq/``

The :ref:`following steps <otucons>` are taken from the MICCA guide, this involves filtering the sequences and building OTUs, biom tables, reference phylogenetic trees, and other diversity analysis. See the :ref:`MICCA pipeline appendix <apmiccapip>` section.