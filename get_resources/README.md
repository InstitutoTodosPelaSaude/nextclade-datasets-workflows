# Generic Nextclade Dataset Workflow

Generic Snakemake workflow for generating Nextclade datasets. Supports both unsegmented viruses (e.g., Zika) and segmented viruses (e.g., Oropouche L, M, S).

## Usage

```bash
# Activate conda environment
conda activate workflow_env

# For Zika
snakemake --configfile configs/zikav/config.yaml --cores 4

# For OROV S segment
snakemake --configfile configs/orov_s/config.yaml --cores 4

# For OROV M segment
snakemake --configfile configs/orov_m/config.yaml --cores 4

# For OROV L segment
snakemake --configfile configs/orov_l/config.yaml --cores 4

# Dry-run (without executing)
snakemake --configfile configs/zikav/config.yaml -n
```

## Structure

```
workflows/
├── Snakefile                 # Main generic workflow
├── workflow_env.yaml         # Conda environment
├── scripts/
│   └── generate_from_genbank.py
├── configs/
│   ├── zikav/
│   │   ├── config.yaml       # Zika configuration
│   │   └── resources/        # auspice_config, pathogen.json, etc.
│   ├── orov_s/
│   │   ├── config.yaml
│   │   └── resources/
│   ├── orov_m/
│   │   ├── config.yaml
│   │   └── resources/
│   └── orov_l/
│       ├── config.yaml
│       └── resources/
└── README.md
```

## Configuration

Each virus/segment has its own `config.yaml`:

| Parameter | Description |
|-----------|-------------|
| `virus_name` | Virus name (used for directories) |
| `dataset_name` | Final dataset name |
| `reference_code` | Reference accession (GenBank) |
| `taxon_id` | NCBI taxon ID |
| `min_length` | Minimum sequence length |
| `max_length` | Maximum length (empty = no limit) |
| `max_seqs` | Maximum number of sequences |
| `allowed_divergence` | Maximum allowed substitutions |
| `rooting` | Rooting method (`mid_point` or accession) |
| `include_parent_features` | Include parent features in GFF3 |

## Outputs

For each virus, the workflow generates:
- `{dataset_name}/` - Directory with dataset files
- `{dataset_name}.zip` - Zipped dataset ready for use
- `test_out/{virus_name}/` - Nextclade test results

## Adding a New Virus

1. Create directory: `configs/{new_virus}/`
2. Create `config.yaml` with parameters
3. Copy/create files in `resources/`:
   - `auspice_config.json`
   - `exclude.txt`
   - `include.txt`
   - `node_clades.tsv`
   - `pathogen.json`
4. Run: `snakemake --configfile configs/{new_virus}/config.yaml --cores 4`
