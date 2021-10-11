# Extract the human ensembl gene catalog to simple tables

This repository extracts a catalog of human genes from the Ensembl database.
It is ideal for situations where you want to represent human genes via stable Ensembl identifiers.
Data is extracted by a series of SQL queries as well as additional transformations in Python using Pandas.
Tables are exported to output branches based on the corresponding Ensembl version in TSV and Parquet format.

## Motivation

NCBI publishes the `Homo_sapiens.gene_info.gz` dataset of human genes with one row per gene.
It includes useful metadata like the gene symbol, synonyms, and chromosome.
However, we weren't able to find a comparable dataset for Ensembl genes (please [let us know](https://github.com/related-sciences/ensembl-genes) if this exists).
Therefore, we combined several SQL queries guided by Biostars answers —
for example to retrieve [symbols](https://www.biostars.org/p/14367/#480311), [alternative sequence allele groups](https://www.biostars.org/p/143956/#144112), and [chromosomes](https://www.biostars.org/p/106355/) —
and from [Open Targets pipelines](https://github.com/opentargets/platform-input-support/blob/b5bf58457ae71a7e32d0dae58340ff5f9d30591d/scripts/ensembl/create_genes_dictionary.py#L46-L78) to extract simplified tabular datasets.

Note that the Ensembl core schema consists of [many tables](https://uswest.ensembl.org/info/docs/api/core/core_schema.html).
There is a chance we have made mistakes and will appreciate any feedback or contributions.
Please use [GitHub Issues](https://github.com/related-sciences/ensembl-genes/issues) for contact.

## Usage

Each release received a corresponding output branch.
For example, see the [`output-104`](https://github.com/related-sciences/ensembl-genes/tree/output-104) branch for datasets generated from Ensembl release 104.

If you'd like to download all files for a specific release,
you can use a command like the following (replacing `104` with the desired release number):

```shell
# clone the relevant output branch to a local directory
git clone --branch=output-104 --depth=1 https://github.com/related-sciences/ensembl-genes.git
# optionally uninitialize git from the data directory
cd ensembl-genes && rm -rf .git
```

Maintainers can create releases for new Ensembl releases running the [export workflow](https://github.com/related-sciences/ensembl-genes/actions/workflows/export.yaml)
(which is a `workflow_dispatch` GitHub Action).

## Development

```shell
# Install the environment
poetry install --no-root

# Update the lock file
poetry update

# Export datasets to output (change 104 to desired release)
poetry run python src/ensembl_genes.py datasets --release=104

# Export notebooks to output (change 104 to desired release)
poetry run python src/ensembl_genes.py notebooks --release=104

# Run tests
pytest

# Set up the git pre-commit hooks.
# `git commit` will now trigger automatic checks including linting.
pre-commit install

# Run all pre-commit checks (CI will also run this).
pre-commit run --all
```

## License

This repository is released under an Apache License 2.0 License (see [LICENSE.md](LICENSE.md)).
Furthermore, output datasets are also released under [CC0 1.0 Universal (CC0 1.0) Public Domain Dedication](https://creativecommons.org/publicdomain/zero/1.0/).

Please familiarize yourself with the [Ensembl data disclaimer](https://m.ensembl.org/info/about/legal/disclaimer.html):

> Ensembl imposes no restrictions on access to, or use of, the data provided and the software used to analyse and present it. Ensembl data generated by members of the project are available without restriction. …
>
> Some of the data and software included in the distribution may be subject to third-party constraints. Users of the data and software are solely responsible for establishing the nature of and complying with any such restrictions.
>
> The European Molecular Biology Laboratory's European Bioinformatics Institute (EMBL-EBI) provides this data and software in good faith, but make no warranty, express or implied, nor assume any legal liability or responsibility for any purpose for which they are used.
