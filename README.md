# ParcelMate

ParcelMate is a Python toolkit for analyzing neural network representations through the lens of brain-like network analysis. It provides tools for studying the organization and dynamics of hidden representations in language models using methods inspired by neuroscience.

## Overview

ParcelMate implements a pipeline for:
1. Extracting hidden state timecourses from language models
2. Computing functional connectivity between hidden units
3. Probabilistically parcellating the network into subnetworks
4. Identifying stable, recurring subnetworks
5. Studying the effects of network interventions

## Quick Start

1. Clone the repository:
```bash
git clone [repository-url]
cd parcelmate
```

2. Run the analysis pipeline:
```bash
python -m parcelmate.bin.main [config_path] [options]
```

### Command Line Options

- `config_path`: Path to configuration file (optional)
- `-s, --steps`: Space-delimited list of steps to run, or 'all' (default: 'all')
- `-O, --overwrite`: Recompute all outputs, even if they already exist

### Example Usage

Run all steps with default settings:
```bash
python -m parcelmate.bin.main
```

Run specific steps:
```bash
python -m parcelmate.bin.main -s connectivity parcellation
```

Use a custom configuration:
```bash
python -m parcelmate.bin.main config.json
```

## Pipeline Steps

The main script supports the following steps:
- `connectivity`: Generate a a "connectome" (correlation matrix) between all pairs of hidden activations in the model from a sample of text from some dataset/domain.
- `parcellation`: Probabilistically parcellate the connectome into subnetworks (sets of hidden units) by repeatedly clustering the hidden units using k-means, greedily aligning the clusterings into a common space and averaging out to produce a probabilistic assignment of units to networks.
- `subnetwork_extraction`: Identify stable subnetworks that are present for all different domains.
- `plot_connectivity`: Visualize connectivity matrices.
- `plot_parcellation`: Visualize parcellation results.
- `plot_stability`: Visualize network stability.
- `subnetwork_knockout`: Knockout stable networks and compute resulting connectivity.

## Configuration

Configuration can be provided via a JSON file with the following structure:
```json
{
    "output_dir": "results",
    "connectivity": {
        "model_name": "gpt2",
        "domains": ["wikitext", "bookcorpus"],
        "seq_len": 1024,
        "n_tokens": 100000
    },
    "parcellation": {
        "n_networks": 50,
        "n_samples": 100
    }
}
```
