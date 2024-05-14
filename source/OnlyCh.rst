.. _onlych:

Only children
=============

Analizying only children
------------------------

We separate the data to keep only the boys and girls from birth to two years of age, with which we do the statistical analysis of the comparison between groups and the analysis of biodiversity.

Step by step of procedure
#########################

The steps to follow are those:

    :ref:`1. Separate the data from the OTU tables. <split>`

    :ref:`2. Do the biodiversity analysis again. <diversity>`


.. _split:

1. Splitting samples from OTU tables
------------------------------------
Only the OTUs of boys and girls are obtained. For this, a new BIOM table is built `filtersamplesfromotutable.py <http://qiime.org/scripts/filter_samples_from_otu_table.html>`_ where the data of the mothers are no longer included for the analysis.

We obtain a new BIOM table that includes only the samples that interest us

    ``filter_samples_from_otu_table.py -i otu_table_filtered-0.00005.biom -o otu_table_filtered-0.00005_childs_only.biom -m MexBC-9c.csv -s 'SampleType:child'``

.. _diversity:

2. Diversity analysis (modified)
--------------------------------

``core_diversity_analyses.py --recover_from_failure -c "Family,Intervals,BirthType,Nutrition" -i $PWD/open_ref_otus_97/otu_table_filtered-0.00005_childs_only.biom -m $PWD/../MexBC-9c.csv -t $PWD/open_ref_otus_97/rep_set.tre -e 10094 -o $PWD/diversity_analysis_filtered_only_child_new_MexBC-9c/open_ref -a -O 4``
