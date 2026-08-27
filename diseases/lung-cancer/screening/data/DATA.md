# Data

Not committed. Reproduce with the commands below.

Both files are derived from sources whose licences do not compose with this
repository's Apache-2.0: ChEMBL is CC BY-SA 3.0 (ShareAlike is copyleft), and
ZINC carries its own terms. They are also fully reproducible from a short query,
so reproduction beats redistribution.

Verify your extraction against the `sha256` below before comparing results with
anyone else. Two people silently working from different data is the worst kind
of bug in a project like this.

## actives.csv (3,944 rows)

Source: ChEMBL 37, target CHEMBL6136 (Lysine-specific histone
demethylase 1A, Homo sapiens, SINGLE PROTEIN).
License: CC BY-SA 3.0. Not redistributed here.

Download: https://ftp.ebi.ac.uk/pub/databases/chembl/ChEMBLdb/latest/chembl_37_sqlite.tar.gz

Query: see notebooks/01_extract_actives.ipynb  (NOT YET WRITTEN)
Filters: pchembl_value IS NOT NULL, standard_relation = '=',
         assay_type = 'B'. Deduped by canonical SMILES, median pChEMBL.

Schema: smiles (str), pchembl (float)
sha256: f00b059a486896775df1c932a44d0b0772588fa00238eeed221d927ce8fa869f

## library.csv (99,990 rows)

Source: ZINC-20 2D tranches, MW/logP windows B-C x A-D.  (UNCONFIRMED —
the file this checksum describes is 99,990 rows / 7.1 MB, not the 812,156-row
build. Confirm which tranches actually produced it before relying on this.)
License: ZINC terms of use. Not redistributed here.

Download and assembly: see notebooks/02_build_library.ipynb  (NOT YET WRITTEN)
Schema: smiles (str), catalog_id (str)
sha256: c12e8f3f60bd7d78997b7df982fb43ecc43f6ca55e71cd7121b1dfd9d59f58ca

Note: `catalog_id` values in this build are ChEMBL IDs (e.g. CHEMBL6329), not
ZINC IDs — another reason to confirm the provenance above.
