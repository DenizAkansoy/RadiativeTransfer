# Companion Cast Shadows in Protoplanetary Discs

This repository contains the code used to produce the results presented in our paper ([link to paper]), which investigates how shadows induced by orbiting companions affect the structure and observable properties of protoplanetary discs.

## Requirements

To run this code, you'll need to install both the [RADMC-3D](https://github.com/dullemond/radmc3d-2.0/tree/master) radiative transfer package and its Python interface, [radmc3dPy](https://www.ita.uni-heidelberg.de/~dullemond/software/radmc-3d/manual_rmcpy/index.html). Additionally, ensure that any other dependencies listed in `requirements.txt` are installed.

## Repository Contents

The code provided here builds upon the example scripts from RADMC-3D for a simple flared disc setup ([\run_ppdisc_analytic_1](https://github.com/dullemond/radmc3d-2.0/tree/master/examples/run_ppdisk_analytic_1)), adding a companion object with a set mass and orbital radius embedded within the disc to study its impact.

### Key Components

- **`modules.py`**: Contains functions to calculate and apply a companion-induced gap in the surface density distribution, as well as to embed the companion’s Hill sphere within a 3D dust density grid. For further details on these calculations, please refer to our paper ([link to be added]).

- **`problem_setup.py`**: An adapted version of the pre-existing `problem_setup.py`, which analytically calculates the dust distribution within a flared disc. This modified script includes companion-induced substructures, such as the axisymmetric gap and Hill sphere. It runs in conjunction with `modules.py`; the only user-defined parameters needed here are the companion mass and orbital distance.

- **`run_params.sh`**: A shell script to streamline the process, running all necessary steps to generate the final scattered light images.

## Example Workflow

The `run_params.sh` shell script automates the entire workflow, from setting up the dust density distribution to generating the final scattered light image. Below is an outline of what `run_params.sh` does:

1. **Specify Parameters**: In `run_params.sh`, specify the companion’s mass and orbital distance. These values are then automatically updated in `problem_setup.py`.

2. **Dust Density Distribution Calculation**: 
   - `run_params.sh` calls `problem_setup.py`, which calculates the dust density distribution, including companion-induced substructures (axisymmetric gap and Hill sphere). 
   - During this step, `problem_setup.py` automatically uses functions from `modules.py`, so no additional user input is required.
   - Once completed, `problem_setup.py` generates output files such as `dust_density.inp`, following RADMC-3D's input file requirements (see RADMC-3D documentation for more details).

3. **Thermal Monte Carlo Simulation**:
   - After the dust density distribution is set, `run_params.sh` initiates the thermal Monte Carlo simulation by running the command `radmc3d mctherm`. 
   - This simulation, run for a specific number of photons (hardcoded in `problem_setup.py`), calculates the dust temperature grid, outputting a file called `dust_temperature.dat`.

4. **Image Generation**:
   - Finally, `run_params.sh` runs `radmc3d image` to produce the scattered light image, saved in the file `image.out`.

This workflow allows you to go from initial parameter specification to a final scattered light image in a single, automated process.


## Contributions

This code was developed by Deniz Akansoy and Helen Petrou under the supervision of Dr. Giulia Ballabio and Dr. Anna Penzlin at the Astrophysics Group, Imperial College London, during 2023-2024.
