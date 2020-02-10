# SI-data-thermodiffusion
Input files and scripts to reproduce the results of **Lesher et al. (2020): Iron isotope fractionation at the core-mantle boundary by thermodiffusion**:

In order to run the models, you need to have ASPECT installed on your computer. Installation instructions can be found [here](https://geodynamics.org/cig/software/aspect/aspect-manual.pdf#subsection.3.3). However, instead of the development version, you will need to install the branch of ASPECT that was used to run the models in the manuscript, which you can download from github with the following command

```git clone https://github.com/jdannberg/aspect.git```

(instead of the `git clone` command listed in the ASPECT manual [installation instructions](https://geodynamics.org/cig/software/aspect/aspect-manual.pdf#subsection.3.3) under point 3.3.3. Apart from that, you can follow the instructions exactly as listed in the manual. 

All model outputs can be visualized using for example the data analysis and visualization application [ParaView](https://www.paraview.org/). 

There are three sets of models we ran for this study:

#### Benchmark: Reproducing the experimental results ####

The input files for these models are located in the the [reproducing-the-experimental-setup](reproducing-the-experimental-setup/) folder.
They can be used to reproduce the ASPECT models shown in Figure S5. 
There are three different input files, with their names giving the value of alpha as listed in the caption of Figure S5. 

The models can be run with the command

```/path/to/aspect/executable/in/build/directory input-file.prm```

(where `/path/to/aspect/executable/in/build/directory` should be the path to the aspect executable on your system, 
and `input-file.prm` should be replaced by the name of one of the three input files, for example `SOR-27-alpha-2.8.prm`.)

#### 1-D Analysis of fractionation in a stationary boundary layer over time ####

The input files for these models are located in the the [1D-analysis-of-fractionation-near-CMB](1D-analysis-of-fractionation-near-CMB/) folder. 
They can be used to reproduce the ASPECT models shown in Figure S8.
These models assume that iron can only diffuse up to 10 km into the lowermost mantle, so in order to reproduce these models, you will have to 
change and recompile the code. The required changes are given in the patch file [patch1.patch](1D-analysis-of-fractionation-near-CMB/patch1.patch)
and can be applied by copying this file into your aspect source directory and then executing

```patch -p1 < patch1.patch```

You will then need to recompile ASPECT. 

You can run the models using the [run_all_models](1D-analysis-of-fractionation-near-CMB/run_all_models) script. 
The script assumes that you have added an alias for ASPECT in your .bashrc file, so that you can run ASPECT by typing `aspect input-file.prm`. 
If that's not the case, you will either need to add the following line to the end of your `~/.bashrc` file

```alias aspect=/path/to/aspect/executable/in/build/directory```

(replacing `/path/to/aspect/executable/in/build/directory` by the path to the aspect executable on your system; you will need to open a new terminal for this change to take effect)

or you will need to change line 28 in the [run_all_models](1D-analysis-of-fractionation-near-CMB/run_all_models) script
by replacing `aspect` with the full path to the aspect executable on your system. 

Then you can run the models with the command

```bash run_all_models```

This will create one output folder for each of the 1-D models shown in the manuscript. 


#### 2-D ASPECT simulations ####

The input files for these models are located in the the [2D-fractionation-models](2D-fractionation-models/) folder. 
For computing the fractionation of iron isotopes, the models assume that liquids from the core can infiltrate into 
the mantle as long as they are above their solidus (but they also track the motion of thin layers with 10/50/100 km thickness
starting out just above the core-mantle boundary to illustrate where core liquid ends up in case it can not infiltrate into
the mantle over longer distances). 
Consequently, in case you have made the changes decribed above in the section about running the 1-D model, you have to revert them 
(for example by running the command `git checkout ./source/simulator/assemblers/advection.cc` in your ASPECT directory)
and you will have to recompile ASPECT. 

There are two input files: The file [fractionation_k8.5_D2.5e-9.prm](2D-fractionation-models/fractionation_k8.5_D2.5e-9.prm) 
reproduces the model shown in Figure S9 top (with a thermal conductivity k = 8.5 W/m/K and diffusivity D<sub>Fe</sub> = 2.5e-9 m<sup>2</sup>/s). 
The file [fractionation_k4_D1e-8.prm](2D-fractionation-models/fractionation_k4_D1e-8.prm) 
reproduces the model shown in Figure S9 bottom (with a thermal conductivity k = 4 W/m/K and diffusivity D<sub>Fe</sub> = 1e-8 m<sup>2</sup>/s).

These models have more than 4 million cells and we ran them on 192 cores, so you will likely need access to a powerful 
computer to rerun these models. 


