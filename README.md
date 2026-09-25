# Algebraic filters reconstruction for X-ray scattering tensor tomography

[![Paper](<https://img.shields.io/badge/Optics%20Express-10.1364%2FOE.597352-blue>)](https://doi.org/10.1364/OE.597352)
[![arXiv](https://img.shields.io/badge/arXiv-2607.19124-b31b1b)](https://arxiv.org/abs/2607.19124)
[![CI](https://github.com/mesqant/algebraic-filters-tt/actions/workflows/ci.yml/badge.svg)](https://github.com/mesqant/algebraic-filters-tt/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Companion notebooks to

> A. M. Antunes, D. M. Pelt, and K. J. Batenburg,
> **Fast reconstruction of tensor tomographic X-ray scattering data for real-time applications**,
> *Optics Express* (2026). [doi:10.1364/OE.597352](https://doi.org/10.1364/OE.597352) · [arXiv:2607.19124](https://arxiv.org/abs/2607.19124)

The paper computes *algebraic filters* (AF) that make a single filtering + back-projection step approximate $k$ Landweber
iterations for different types of X-ray scattering-based tensor tomography. The method separates the tomographic projector $\mathbf{\overline{W}}$ from a
view-dependent mixing operator $\mathbf{Y}$ ($\mathbf{A} = \mathbf{\overline{W}}\mathbf{Y}$), so it applies to any tensor representation and scattering
modality. These notebooks give a complete implementation for the simulated 'M' phantom: representation changes,
forward models, filter computation and reconstruction.

![Central slice of the six rank-2 tensor entries: ground truth, 50 Landweber iterations, and the algebraic filters reconstruction](docs/comparison_tensor.png)

## Notebooks

| Notebook                                                       | Content                                                                                                 | Paper                | Run                                                                                                                                                                                                                                                                                                                                                                                                      |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [01 · Representations](notebooks/01_representations.ipynb)     | Spherical harmonics → directional (7 directions) → rank-2 tensor                                      | Sec. 4.1, Figs. 2–3 | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/mesqant/algebraic-filters-tt/blob/main/notebooks/01_representations.ipynb) [![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg)](https://nbviewer.org/github/mesqant/algebraic-filters-tt/blob/main/notebooks/01_representations.ipynb)     |
| [02 · Forward models](notebooks/02_forward_models.ipynb)       | Projector $\mathbf{\overline{W}}$, mixing operators $\mathbf{Y}$ for the three representations, simulated projections  | Sec. 2.2, 3.2        | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/mesqant/algebraic-filters-tt/blob/main/notebooks/02_forward_models.ipynb) [![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg)](https://nbviewer.org/github/mesqant/algebraic-filters-tt/blob/main/notebooks/02_forward_models.ipynb)       |
| [03 · Algebraic filters](notebooks/03_algebraic_filters.ipynb) | Step size (power method), Landweber, filters (Algorithm 1), AF reconstruction (Algorithm 2), comparison | Sec. 3, Figs. 4–5   | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/mesqant/algebraic-filters-tt/blob/main/notebooks/03_algebraic_filters.ipynb) [![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg)](https://nbviewer.org/github/mesqant/algebraic-filters-tt/blob/main/notebooks/03_algebraic_filters.ipynb) |

Run the notebooks in order: each one saves its results in `data/` for the next one. The committed outputs come from a
complete GPU run (NVIDIA RTX 4070 Super).

## Running the notebooks

The notebooks use a GPU (via [CuPy](https://cupy.dev) and mumott's CUDA projector) when one is available and fall
back to the CPU otherwise.

| Where                                        | Hardware                                                   | What runs                                                                                                          |
| -------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Google Colab** (badges in the table) | Free GPU: pick *Runtime → Change runtime type → T4 GPU* | Full size. The first cell clones this repository and installs mumott 2.3 and CuPy: mumott 2.1, used for the paper, does not install on Colab's Python, and mumott 2.3's slightly different projector changes results by a few percent.                             |
| **Locally**                            | CPU or NVIDIA GPU                                          | Full size, as in the paper, on a GPU or on the CPU (notebook 03 then takes ~12 min on 2 cores). For a quicker look, set `small_run = True` in notebook 02 (every 4th view). |

Local installation with conda:

```bash
git clone https://github.com/mesqant/algebraic-filters-tt
cd algebraic-filters-tt
conda env create -f environment.yml
conda activate algfilters-tt
pip install cupy-cuda12x   # optional, for NVIDIA GPUs (use cupy-cuda11x for CUDA 11)
jupyter lab notebooks/
```

or with pip: `pip install -r requirements.txt`.

The pinned versions follow the environment used for the paper (Python 3.11, NumPy 1.26, SciPy 1.16, mumott 2.1).
The original code used mumott 2.1, which requires **SciPy to stay below 1.17** due to the usage of `scipy.special.sph_harm`, which SciPy 1.17 removes.

## Data

Notebook 01 downloads the simulated 'M' tensor phantom (180 MB) from Nielsen et al.,
[doi:10.5281/zenodo.7673985](https://doi.org/10.5281/zenodo.7673985). The experimental datasets of Sec. 4.2 are not
part of this repository. The trabecular bone SASTT dataset and the PMMA box with carbon fibre
bundles GITT dataset are publicly available; see refs. 38–39 and the *Data availability*
section of the paper.

## Citation

If you use this code, please cite the paper:

```bibtex
@article{antunes2026fast,
  title   = {Fast reconstruction of tensor tomographic {X}-ray scattering data for real-time applications},
  author  = {Antunes, Andr{\'e} M. and Pelt, Dani{\"e}l M. and Batenburg, K. Joost},
  journal = {Optics Express},
  year    = {2026},
  doi     = {10.1364/OE.597352}
}
```

GitHub's *Cite this repository* button (from [`CITATION.cff`](CITATION.cff)) gives the same reference.

## Acknowledgements

This work was funded by Horizon Europe through the MSCA Doctoral Network RELIANCE (grant no. 101073040).
The tomographic projector and spherical harmonic utilities come from [mumott](https://mumott.org).

This README and other utilities (including code utilities) were prepared with the help of Claude Opus 5.5. All non-original code was scrutinized by the owner of the repository.
## License

[MIT](LICENSE)
