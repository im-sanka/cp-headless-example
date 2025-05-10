# 🧪 CellProfiler Headless Setup on Apple Silicon M-Series (M1/ M2/ etc.)

This guide walks you through installing and running CellProfiler in headless mode on Apple Silicon (M1, M2, etc.) Macs using Rosetta 2, Conda, and Mamba.


## 🚀 Why This?
Running CellProfiler on Apple Silicon isn't straightforward—many run into broken dependencies or unsupported architectures. This guide makes the process smoother.


---

## 🛠️ Prerequisites

```
- Mac with Apple Silicon (M series)
- Basic terminal familiarity
- Rosetta 2
- Miniconda (recommended)
```

---

## 🧰 Setup Instructions

### 1. Install Rosetta 2

```bash
/usr/sbin/softwareupdate --install-rosetta --agree-to-license
```

```
To always run Terminal using Rosetta:

1. Duplicate Terminal from /Applications/Utilities/
2. Rename it to Terminal (Rosetta)
3. Right-click > Get Info > Check "Open using Rosetta"
```

```bash
# To temporarily emulate Intel in Terminal:
arch -arch x86_64 /bin/zsh

# To switch back to native arm64:
arch -arch arm64 /bin/zsh

# Confirm architecture:
uname -m  # should return x86_64 under Rosetta
```

---

### 2. Install Miniconda

```
Download and install the Apple Silicon version from: https://docs.conda.io/en/latest/miniconda.html
```

```bash
conda init zsh
source ~/.zshrc
```

---

### 3. Install Mamba

```bash
conda activate base
conda install -n base -c conda-forge mamba
mamba shell init --shell zsh --root-prefix="$HOME/miniconda3"
source ~/.zshrc
```

---

### 4. Create Environment File

```bash
# Create and open environment.yaml (or use a text editor)
vi environment.yaml
```

```yaml
# Copy the following into environment.yaml
name: cellprofiler
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.8
  - cellprofiler=4.2.1
  - openjdk
  - python.app
  - h5py
  - mysqlclient
  - pillow
  - numpy<1.24
  - pip
  - pip:
      - typeguard==2.12.1
```

---

### 5. Create and Activate the Environment

```bash
export CONDA_SUBDIR=osx-64
mamba env create -f environment.yaml
mamba activate cellprofiler
```

---

## ✅ Usage

### Headless Mode

```bash
cellprofiler -c -r -p your_pipeline.cppipe -i ./images
```

### GUI Mode

```bash
cellprofiler
```

---

## 📁 Included Resources

```
- example_pipeline.cppipe: Sample pipeline file
- images/: Folder with some sample input images of droplets
```

---

## 💡 Tips

```
- Try building pipelines in the GUI first, then switch to headless for automation.
- Terminal logs help in debugging and integrate well with tools like Snakemake/Nextflow.
- Use uname -m to check whether you're running under Rosetta (x86_64) or native (arm64).
```

---

## 🙋 About
Author: Immanuel Sanka

```
Happy analyzing! 🎉
```
