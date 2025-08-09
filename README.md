# AnthroAb: Antibody Humanization Language Model

```
              █████  ███    ██ ████████ ██   ██ ██████   ██████      █████  ██████  
             ██   ██ ████   ██    ██    ██   ██ ██   ██ ██    ██    ██   ██ ██   ██ 
             ███████ ██ ██  ██    ██    ███████ ██████  ██    ██ ██ ███████ ██████  
             ██   ██ ██  ██ ██    ██    ██   ██ ██   ██ ██    ██    ██   ██ ██   ██ 
             ██   ██ ██   ████    ██    ██   ██ ██   ██  ██████     ██   ██ ██████
```

AnthroAb is a human antibody language model based on RoBERTa, specifically trained for antibody humanization tasks.

## Features

- **Antibody Humanization**: Predict humanized versions of antibody sequences
- **CLI Batch Processing**: Command-line interface for processing multiple sequences from FASTA files
- **Sequence Infilling**: Fill masked positions with human-like residues
- **Mutation Suggestions**: Suggest humanizing mutations for frameworks and CDRs
- **Embedding Generation**: Create vector representations of residues or sequences
- **Dual Chain Support**: Separate models for Variable Heavy (VH) and Variable Light (VL) chains

## Installation

```bash
# Install from PyPI 
conda create -n anthroab python=3.10
conda activate anthroab
pip install anthroab

# Or install from source
git clone https://github.com/nagarh/AnthroAb
cd AnthroAb
pip install -e .
```

## Quick Start

### Antibody Sequence Humanization

```python
import anthroab

# Humanize a heavy chain sequence
vh_sequence = "'**QLV*SGVEVKKPGASVKVSCKASGYTFTNYYMYWVRQAPGQGLEWMGGINPSNGGTNFNEKFKNRVTLTTDSSTTTAYMELKSLQFDDTAVYYCARRDYRFDMGFDYWGQGTTVTVSS"
humanized_vh = anthroab.predict_masked(vh_sequence, 'H')
print(f"Humanized VH: {humanized_vh}")

# Humanize a light chain sequence
vl_sequence = "DIQMTQSPSSLSASV*DRVTITCRASQSISSYLNWYQQKPGKAPKLLIYSASTLASGVPSRFSGSGSGTDF*LTISSLQPEDFATYYCQQSYSTPRTFGQGTKVEIK"
humanized_vl = anthroab.predict_masked(vl_sequence, 'L')
print(f"Humanized VL: {humanized_vl}")
```

### CLI Batch Processing

AnthroAb provides a command-line interface for batch processing FASTA files containing multiple antibody sequences.

#### Installation & Usage

After installing AnthroAb, you can use the CLI directly:

```bash
anthroab -i input.fasta -o output.fasta --humanize
```

#### Input Format

Your input FASTA file should follow this naming convention:
- Heavy chains: `{antibody_name}_VH`
- Light chains: `{antibody_name}_VL`
- Use `*` to mark positions for humanization

**Example input.fasta:**
```fasta
>seq1_VH
**QLV*SGVEVKKPGASVKVSCKASGYTFTNYYMYWVRQAPGQGLEWMGGINPSNGGTNFNEKFKNRVTLTTDSSTTTAYMELKSLQFDDTAVYYCARRDYRFDMGFDYWGQGTTVTVSS
>seq1_VL
DIQMTQSPSSLSASV*DRVTITCRASQSISSYLNWYQQKPGKAPKLLIYSASTLASGVPSRFSGSGSGTDF*LTISSLQPEDFATYYCQQSYSTPRTFGQGTKVEIK
>seq2_VH
**QLV*SGVEVKKPGASVKVSCKASGYTFTNYYMYWVRQAPGQGLEWMGGINPSNGGTNFNE**KNRVTLTTDSSTTTAYMELKSLQFDDTA*YYCARRDYRFDMGFDYWGQGTTVTVSS
>seq2_VL
DIQMTQ*PSSLSASV*DRVTITCRASQSISSYL**YQQKPGKAPKLLIYSASTLASGVPSRFSGSGSGTDF*LTISSLQPEDFATYYCQQSYSTPRTFGQGTKVEIK
```

#### Features

- **Batch Processing**: Process multiple antibody sequences in one command
- **Progress Tracking**: Real-time progress bar showing processing status
- **Pair Validation**: Automatically detects VH/VL pairs and warns about missing chains
- **Error Handling**: Gracefully handles processing errors while continuing with other sequences
- **Model Selection**: Automatically uses appropriate models (VH model for heavy chains, VL model for light chains)

#### CLI Options

```bash
anthroab --help
```

**Required Arguments:**
- `-i, --input`: Input FASTA file with sequences to humanize
- `-o, --output`: Output FASTA file for humanized sequences  
- `--humanize`: Enable humanization using "predict_masked" mode

**Example Commands:**
```bash
# Basic usage
anthroab -i antibodies.fasta -o humanized_antibodies.fasta --humanize

# Show help
anthroab --help
```

## Model Details

### Architecture
- **Base Model**: RoBERTa (trained from scratch)
- **Architecture**: RobertaForMaskedLM
- **Model Type**: Masked Language Model for antibody sequences

### Model Specifications
- **Hidden Size**: 768
- **Number of Layers**: 12
- **Number of Attention Heads**: 12
- **Intermediate Size**: 3072
- **Max Position Embeddings**: 192 (VH), 145 (VL)
- **Vocabulary Size**: 25 tokens
- **Model Size**: ~164 MB per model

### Available Models
- **VH Model**: `hemantn/roberta-base-humAb-vh` - For Variable Heavy chains
- **VL Model**: `hemantn/roberta-base-humAb-vl` - For Variable Light chains



## Citation

If you use AnthroAb in your research, please cite:

```bibtex
@misc{anthroab,
  author = {Hemant Nagar},
  title = {AnthroAb: Human Antibody Language Model for Humanization},
}
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Acknowledgments

**Note**: This codebase and API design are adopted from the [Sapiens](https://github.com/Merck/Sapiens) model by Merck.AnthroAb maintains the same interface and functionality as Sapiens but utilizes a RoBERTa-base model trained on human antibody sequences from the OAS database (up to year 2025) for antibody humanization.

### Original Sapiens Citation
> David Prihoda, Jad Maamary, Andrew Waight, Veronica Juan, Laurence Fayadat-Dilman, Daniel Svozil & Danny A. Bitton (2022) 
> BioPhi: A platform for antibody design, humanization, and humanness evaluation based on natural antibody repertoires and deep learning, mAbs, 14:1, DOI: https://doi.org/10.1080/19420862.2021.2020203

## Related Projects

- **[Sapiens](https://github.com/Merck/Sapiens)**: Original antibody language model by Merck (this codebase is based on Sapiens)
- **[BioPhi](https://github.com/Merck/BioPhi)**: Antibody design and humanization platform
- **[OAS](https://opig.stats.ox.ac.uk/webapps/oas/)**: Observed Antibody Space database

## Support

For questions, issues, or contributions, please open an issue on the GitHub repository. 