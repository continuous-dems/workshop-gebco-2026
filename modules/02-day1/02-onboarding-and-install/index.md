---
title: "2 - Onboarding and Installation"
---

The first goal of the workshop is to get everyone into a consistent working environment. CUDEM workflows depend on several geospatial and scientific Python libraries, so we want all participants using the same `cudem` conda environment before we begin running DEM examples.

By the end of this section, participants should be able to:

- Join the Zulip chat space for technical support
- Install and open a conda terminal
- Activate the workshop `cudem` conda environment
- Confirm that the core CUDEM tools are installed
- Run basic help and version checks

Before we build any DEMs, we need to make sure everyone is working from the same software environment. The first part of the workshop will focus on activating the `cudem` environment and confirming that the main tools are available.

If you have a machine-specific installation issue during the workshop, use the Zulip chat space to message the workshop support team. The goal is to get everyone to a stable enough environment to run the Fiji test DEM. If someone has a deeper issue with their computer, we will use shared example outputs so they can still follow along with the workflow discussion.

---

## 1. Join the Zulip Chat Space

We will use the Zulip chat space for technical support during the workshop. This gives participants a place to ask questions, report installation issues, share screenshots, and get help from the workshop team.

Please join the chat space before continuing with the installation steps:

[CUDEM Zulip chat space](https://cudem.zulipchat.com)

Use the workshop stream:

```text
#CUDEM Workshop - GEBCO/NIPPON Scholars > GEBCO/Nippon Scholars
```

---

## 2. Install and Open a Miniforge Terminal

Install Miniforge for your operating system:

<https://conda-forge.org/download/>

After installation, open a Miniforge terminal.

On Windows, open:

```text
Miniforge Prompt
```

On macOS or Linux, open a terminal.

All workshop commands should be run from the Miniforge Prompt or terminal unless otherwise noted.

---

## 3. Create the CUDEM Environment

Run the following commands to create and activate the workshop environment:

```bash
conda create -n cudem -c conda-forge python=3.12
conda activate cudem
conda install -c conda-forge libgdal-netcdf copernicusmarine pygmt globato ivert
```

This creates a separate `cudem` environment instead of installing workshop tools into the base environment.

The install command provides the core tools used during the workshop:

```text
fetchez
transformez
globato
ivert
```

Activate the environment:

```bash
conda activate cudem
```

Your prompt should now begin with:

```text
(cudem)
```

---

## 4. Install HTDP

`transformez` uses NOAA/NGS HTDP for horizontal time-dependent positioning and frame transformations. HTDP must be installed separately before running some CUDEM transformation workflows.

Download HTDP here:

<https://github.com/noaa-ngs/HTDP/archive/refs/tags/v3.5.0.zip>

Follow the HTDP installation instructions for your operating system.

---

## 5. Copernicus Marine Credentials

Some CUDEM recipes use the Copernicus Marine data module. If a recipe accesses Copernicus Marine data, the module needs valid Copernicus Marine credentials.

Create a free Copernicus Marine account:

<https://data.marine.copernicus.eu/register>

After registering, create a `.netrc` file so the data module can log in on the backend.

### Windows

On Windows, open the `.netrc` file in your user home directory:

```bash
notepad %USERPROFILE%\.netrc
```

Add the following entry, replacing `YOUR_USERNAME` and `YOUR_PASSWORD` with your Copernicus Marine credentials:

```text
machine data.marine.copernicus.eu
login YOUR_USERNAME
password YOUR_PASSWORD
```

Save and close the file.

Confirm the file exists:

```bash
dir %USERPROFILE%\.netrc*
```

If the file was accidentally saved as `.netrc.txt`, rename it to remove the extension:

```bash
ren %USERPROFILE%\.netrc.txt .netrc
```

### macOS / Linux

On macOS or Linux, create or edit the `.netrc` file in your home directory:

```bash
nano ~/.netrc
```

Add the following entry, replacing `YOUR_USERNAME` and `YOUR_PASSWORD` with your Copernicus Marine credentials:

```text
machine data.marine.copernicus.eu
login YOUR_USERNAME
password YOUR_PASSWORD
```

Save the file, then update the file permissions:

```bash
chmod 600 ~/.netrc
```

---

## 6. (Optional) NASA EarthData Credentials

The `ivert` tool uses NASA's EarthData tools to fetch and subset ICESat-2 data. We will provide some pre-computed data for use during the workshop, so this step is optional. However, if you would like to perform your own validations later, you will need to register for a free NASA EarthData account and put your credentials into your `.netrc` file.

Create a free NASA EarthData account:

<https://urs.earthdata.nasa.gov/users/new>

After registering, add an entry to your `.netrc` file so `ivert` can log in on the backend.

### Windows

On Windows, open the `.netrc` file again in your user home directory:

```bash
notepad %USERPROFILE%\.netrc
```

Add the following entry, replacing `YOUR_USERNAME` and `YOUR_PASSWORD` with your NASA EarthData credentials. This can go immediately after your Copernicus Marine entry from above:

```text
machine urs.earthdata.nasa.gov
login YOUR_USERNAME
password YOUR_PASSWORD
```

Save and close the file.

### macOS / Linux

On macOS or Linux, create or edit the `.netrc` file in your home directory:

```bash
nano ~/.netrc
```

Add the following entry, replacing `YOUR_USERNAME` and `YOUR_PASSWORD` with your NASA EarthData credentials. This can go immediately after your Copernicus Marine entry from above:

```text
machine urs.earthdata.nasa.gov
login YOUR_USERNAME
password YOUR_PASSWORD
```
---

## 7. Run Setup Checks

After installation, confirm the environment and tools are working.

Make sure the `cudem` environment is active:

```bash
conda activate cudem
```

Check Python:

```bash
python --version
```

Expected:

```text
Python 3.12.x
```

Check GDAL:

```bash
gdalinfo --version
```

Check the CUDEM tools:

```bash
fetchez --help
transformez --help
globato --help
ivert --help
```

If these commands print help text, the environment is ready for the Fiji test DEM.

---

## 8. Next Step

Once everyone has a working `cudem` environment, we will move to the Fiji test DEM exercise.

The next module will use `globato` to run a small Fiji/Suva-area DEM workflow and generate a shared output that we can inspect together.
