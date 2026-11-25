<h1 align="center">HCP Pipelines: structural preprocessing</h1>

<p align="center">
  HCP-style structural preprocessing (PreFreeSurfer, FreeSurfer and PostFreeSurfer) from a T1-weighted and a T2-weighted image, with the HCP Pipelines BIDS-App.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.465"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.465-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="MIT license"></a>
</p>

> 📍 **This branch, `v4.1.3`, is the brainlife.io App HCP Pipelines - Structural preprocessing** ([doi:10.25663/brainlife.app.465](https://doi.org/10.25663/brainlife.app.465)).

## 🧠 At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Runs the HCP Pipelines 4.1.3 structural preprocessing (PreFreeSurfer, FreeSurfer, PostFreeSurfer) on T1- and T2-weighted images with the HCP BIDS-App.**

Converts the inputs to BIDS and runs the HCP Pipelines BIDS-App (bids/hcppipelines:v4.1.3-1) for the chosen stages. Needs at least one T1w and one T2w image. If a FreeSurfer 6 output is given, the FreeSurfer stage is skipped; FreeSurfer 7 output is not supported.

| | |
|---|---|
| 📥 **Inputs** | `neuro/anat/t1w` · `neuro/anat/t2w` · `neuro/freesurfer` (optional) |
| 📤 **Outputs** | `neuro/hcp/freesurferpost` |
| 🐳 **Container** | `bids/hcppipelines:v4.1.3-1` |
| ⚙️ **Resources** | 8 CPUs · 16 GB memory · 15 h walltime |
| 🔑 **Requires** | `FREESURFER_LICENSE` |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.465](https://doi.org/10.25663/brainlife.app.465) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `0` | `neuro/anat/t1w` | yes | T1 image |
| `1` | `neuro/anat/t2w` | yes | T2 image |
| `2` | `neuro/freesurfer` (!v7) | no | Freesurfer output. With this input, the FreeSurfer step will be skipped. WARNING: FreeSurfer 7.X is not currently supported due to poor quality surface reconstructions. Please provide a FreeSurfer 6.0 output. (NOTE: do not provide this input when using ABCD data) |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `output` | `neuro/hcp/freesurferpost` | HCP structural preprocessing output folder (T1w, MNINonLinear and the FreeSurfer subject directory). |

<details>
<summary><b>⚙️ Parameters</b> (4)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `stage` | string | `PreFreeSurfer FreeSurfer PostFreeSurfer` | Which stages to run. Space separated list. (at the moment only the default [PreFreeSurfer FreeSurfer PostFreeSurfer] can be run). If you input a freesurfer folder, the FreeSurfer step will be skipped. |
| `processing_mode` | enum | `legacy` | Control HCP-Pipeline mode. 'hcp' (HCPStyleData): require T2w and fieldmap modalities. 'legacy' (LegacyStyleData): always ignore T2w and fieldmaps. 'auto': use T2w and/or fieldmaps if available. (at the moment only 'legacy' can be run). Options: `hcp`, `legacy`, `auto`. |
| `notalcheck` | boolean | `false` | If you are seeing "ERROR: Talairach failed", you might be able to work around this by setting this option so that freesurfer will skip this process. If you set this option, you will need to make sure that your image is well aligned to the 3 primary axis. |
| `skipbidsvalidation` | boolean | `false` | Skip the BIDS validation of input data. Try this option if your data is not BIDS compatible but you'd like to go ahead and process it through this App anyway. |

</details>

<!-- brainlife-app:end -->

## 🚀 Running the App

This App is a brainlife.io wrapper for the [HCP Pipelines](https://github.com/Washington-University/HCPpipelines), the tools used to process MRI images for the [Human Connectome Project](https://www.humanconnectome.org/). It implements the structural preprocessing and uses the [HCP Pipelines BIDS-App](https://github.com/BIDS-Apps/HCPPipelines), a container with all dependencies and a run script that parses a BIDS dataset. For this reason the App's version may lag behind the latest HCP Pipelines release.

The registered App runs the `bids/hcppipelines:v4.1.3-1` container from branch [`v4.1.3`](https://github.com/brainlife/app-hcp-pipeline/tree/v4.1.3). The default branch (`master`) and branch `v4.3.0-3` try the `v4.3.0-3` container and are not registered on brainlife.io.

### ☁️ On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.465](https://doi.org/10.25663/brainlife.app.465), choose a T1-weighted and a T2-weighted image (and optionally a FreeSurfer output) from your project, set the options and press **Execute**. The output is a `neuro/hcp/freesurferpost` dataset archived to your project.

The structural preprocessing has three stages (PreFreeSurfer, FreeSurfer and PostFreeSurfer) and needs at least one T1w and one T2w image. If you provide a FreeSurfer output, the FreeSurfer stage is skipped.

> **FreeSurfer 7 is not supported** because of poor-quality surface reconstructions. Provide a FreeSurfer 6.0 output, or none. Do not provide a FreeSurfer input when using ABCD data.

Options:

- `processing_mode`: `hcp` (HCPStyleData) requires T2w and fieldmaps, `legacy` (LegacyStyleData) always ignores T2w and fieldmaps, `auto` uses them when available. At the moment only `legacy` can be run.
- `stage`: fixed to `PreFreeSurfer FreeSurfer PostFreeSurfer`.
- `notalcheck`: skip FreeSurfer's Talairach check.
- `skipbidsvalidation`: skip BIDS validation of the input data.

### 💻 Locally

`main` expects the brainlife.io execution environment: it converts the inputs to BIDS with `bl2bids` and runs the BIDS-App container with [Singularity/Apptainer](https://apptainer.org/); it also needs [`jq`](https://jqlang.github.io/jq/) and a [FreeSurfer license](https://surfer.nmr.mgh.harvard.edu/registration.html). The config keys it reads are `t1`, `t2`, `fsin` (optional FreeSurfer directory), `stage`, `processing_mode`, `notalcheck` and `skipbidsvalidation`.

To run the HCP Pipelines on your own machine, follow the [HCP Pipelines installation and usage instructions](https://github.com/Washington-University/HCPpipelines/wiki/Installation-and-Usage-Instructions) or the [BIDS-App](https://github.com/BIDS-Apps/HCPPipelines).

## 📦 Outputs

`output/` holds the HCP subject directory written by the BIDS-App:

```text
output/sub-<subject>/
├── T1w/            ACPC-aligned, bias-corrected T1w and T2w images and the FreeSurfer subject directory
└── MNINonLinear/   images and surfaces in MNI space
```

## 🩺 Troubleshooting

- **`app needs minimally a T1w and a T2w. exiting`**: provide both a T1-weighted and a T2-weighted image.
- **`ERROR: Talairach failed`**: set `notalcheck` to skip the Talairach check, and make sure the image is well aligned to the three primary axes.
- **BIDS validation errors**: set `skipbidsvalidation` to process data that is not fully BIDS compatible.
- **FreeSurfer input from FreeSurfer 7**: not supported; provide a FreeSurfer 6.0 output or let the App run FreeSurfer.

## 📚 Citing

> [!TIP]
> If you use this App in your work, please cite the App, the software it builds on and brainlife.io (below). GitHub's **Cite this repository** button gives the App citation in APA and BibTeX.

- Glasser MF, Sotiropoulos SN, Wilson JA, Coalson TS, Fischl B, Andersson JL, Xu J, Jbabdi S, Webster M, Polimeni JR, Van Essen DC, Jenkinson M; WU-Minn HCP Consortium. The minimal preprocessing pipelines for the Human Connectome Project. *NeuroImage* 80:105–124 (2013). [doi:10.1016/j.neuroimage.2013.04.127](https://doi.org/10.1016/j.neuroimage.2013.04.127)
- HCP Pipelines - Structural preprocessing. brainlife.io App. [doi:10.25663/brainlife.app.465](https://doi.org/10.25663/brainlife.app.465)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. Sci Data 6, 69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)
- Fischl. FreeSurfer. *NeuroImage* 62:774-781 (2012). [doi:10.1016/j.neuroimage.2012.01.021](https://doi.org/10.1016/j.neuroimage.2012.01.021)
- Jenkinson et al. FSL. *NeuroImage* 62:782-790 (2012). [doi:10.1016/j.neuroimage.2011.09.015](https://doi.org/10.1016/j.neuroimage.2011.09.015)
- Marcus et al. Informatics and Data Mining Tools and Strategies for the Human Connectome Project. *Frontiers in Neuroinformatics* 5 (2011). [doi:10.3389/fninf.2011.00004](https://doi.org/10.3389/fninf.2011.00004)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)

## 👥 Authors

- Giulia Bertò ([@giulia-berto](https://github.com/giulia-berto))
- Soichi Hayashi ([@soichih](https://github.com/soichih))
- Franco Pestilli ([@francopestilli](https://github.com/francopestilli), project director)

## 💰 Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io is publicly funded, and acknowledging that support helps keep it running. This App was developed with support from Wellcome, the U.S. National Institutes of Health (including the BRAIN Initiative BRAIN CONNECTS program: The Axonal Projectome EXchange, U24NS140384, and the Center for Mesoscale Connectomics, UM1NS132207) and the U.S. National Science Foundation.</sub>

## ⚖️ License

[MIT](LICENSE). The HCP Pipelines, FreeSurfer and FSL are distributed under their own licenses.
