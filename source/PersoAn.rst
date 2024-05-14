Personalized analysis (optional)
================================
Para saber cómo trabajar con una parte de los datos (p. ej. sólo los niños y niñas) revisar el documento :ref:`only_children <onlych>`

BIOM binary to text
-------------------

To convert the binary BIOM table to text format, separated by tabs (tsv), follow the instructions on `biom-format.org <http://biom-format.org/>`_.

Example
-------
As an example, the children's BIOM table (see *only_children* above) is obtained in tab-separated text format.

    ``biom convert -i $PWD/open_ref_otus_97/otu_table_only_children.biom -o $PWD/open_ref_otus_97/otu_table_only_children.tsv --to-tsv``

Count the number of data
------------------------

To find out how many records there are in total in the BIOM table, in this case the number of OTUs in the filtered data*, just count the number of lines (lines) in the .tsv file by subtracting the header line:

    ``wc -l open_ref_otus_97/otu_table_filtered-0.00005_only_children.tsv``

We have to be **720 OTUs**, or to obtain the number directly:

    ``wc -l open_ref_otus_97/otu_table_filtered-0.00005_only_children.tsv | echo "`cut -f1 -d' '`-1" | bc -l``

.. note::
    The children's filtered BIOM table has all OTUs from the original filtered BIOM table, including the mother's OTUs. If no sample of the child presents such OTU, the abundance appears as "0" in all the fields of the table.

If required, a script can be made to discard those OTUs that do not appear in any of the children's samples.