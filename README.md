# ACPYPE

**AnteChamber PYthon Parser interfacE**

A tool based in **Python** to use **Antechamber** to generate topologies for chemical
compounds and to interface with others python applications like CCPN or ARIA.

`acpype` is pronounced as _**ace + pipe**_

Topologies files to be generated so far: CNS/XPLOR, GROMACS, CHARMM and AMBER.

NB: Topologies generated by `acpype/Antechamber` are based on General Amber Force
Field (GAFF) and should be used only with compatible forcefields like AMBER and
its variant.

Several flavours of AMBER FF are ported already for GROMACS (see ffamber:
http://ffamber.cnsm.csulb.edu/) as well as to XPLOR/CNS (see `xplor-nih`:
http://ambermd.org/xplor-nih.html) and [CHARMM](https://www.charmm.org/).

This code is released under **[GNU General Public License V3](https://www.gnu.org/licenses/gpl-3.0.en.html)**.

###### NO WARRANTY AT ALL!!!

It was inspired by:

- `amb2gmx.pl` (Eric Sorin, David Mobley and John Chodera)
  and depends on `Antechamber` and `OpenBabel`

- YASARA Autosmiles:
  http://www.yasara.org/autosmiles.htm (Elmar Krieger)

- `topolbuild` (Bruce Ray)

- `xplo2d` (G.J. Kleywegt)

For Non-uniform 1-4 scale factor conversion (e.g. if using **GLYCAM06**), please cite:

> BERNARDI, A., FALLER, R., REITH, D., and KIRSCHNER, K. N. ACPYPE update for
nonuniform 1–4 scale factors: Conversion of the GLYCAM06 force field from AMBER
to GROMACS. SoftwareX 10 (2019), 100241. doi: 10.1016/j.softx.2019.100241

For `Antechamber`, please cite:
> 1. WANG, J., WANG, W., KOLLMAN, P. A., and CASE, D. A. Automatic atom type and
     bond type perception in molecular mechanical calculations. Journal of Molecular
     Graphics and Modelling 25, 2 (2006), 247–260. doi: 10.1016/j.jmgm.2005.12.005
> 2. WANG, J., WOLF, R. M., CALDWELL, J. W., KOLLMAN, P. A., and CASE, D. A.
     Development and testing of a General Amber Force Field. Journal of Computational
     Chemistry 25, 9 (2004), 1157–1174. doi: 10.1002/jcc.20035

If you use this code, I am glad if you cite:

> SOUSA DA SILVA, A. W. & VRANKEN, W. F.
ACPYPE - AnteChamber PYthon Parser interfacE.
BMC Research Notes 5 (2012), 367 doi: 10.1186/1756-0500-5-367
http://www.biomedcentral.com/1756-0500/5/367

> BATISTA, P. R.; WILTER, A.; DURHAM, E. H. A. B. & PASCUTTI, P. G. Molecular
Dynamics Simulations Applied to the Study of Subtypes of HIV-1 Protease.
Cell Biochemistry and Biophysics 44 (2006), 395-404. doi: 10.1385/CBB:44:3:395

Alan Wilter Sousa da Silva, D.Sc.
Bioinformatician, UniProt, EMBL-EBI
Hinxton, Cambridge CB10 1SD, UK.
http://www.ebi.ac.uk/~awilter

alanwilter _at_ gmail _dot_ com

#### How To Use ACPYPE

##### Introduction

We now have an up to date *webservice* at **http://bio2byte.be/acpype/** (but it
**does not** have the `amb2gmx` funcionality).

To run `acpype`, locally, with its all functionalities, you need **ANTECHAMBER** from package
[AmberTools](http://ambermd.org/) and
[Open Babel](http://openbabel.org/wiki/Main_Page) if your input files are of PDB
format.

However, if one wants `acpype` just to emulate *amb2gmx.pl*, one needs nothing
at all but *[Python](http://www.python.org)*.

There several ways of obtaining `acpype`:

1. Via **[CONDA](https://anaconda.org/search?q=acpype)**:

  ```bash
  conda install -c conda-forge acpype
  ```

  or, if you want to be sure to get the latest (sometimes `conda-forge` channel is still lagging behind)

  ```bash
  conda install -c acpype acpype
  ```

1. Via **[PyPI](https://pypi.org/project/acpype/)**:

  ```bash
  pip install git+https://github.com/alanwilter/acpype.git
  ```

  note that `pip install acpype`, unfortunately, is not picking the original one.

1. By downloading it via `git`:

  ```bash
  git clone https://github.com/alanwilter/acpype.git
  ```

**NB:** Installing via `conda` gives you `AmberTools17` and `OpenBabel2.4`, while
via `pip/git` you get `AmberTools19` and `OpenBabel3` (which lacks `obchiral`,
but this is not critical). Our `AmberTools19` comes with binary `charmmgen` from
`AmberTools17` in order to generate CHARMM topologies.

##### To Test, if doing via `git`

At folder `acpype/test`, type:

```bash
../acpype.py -i FFF.pdb
```

It'll create a folder called *FFF.acpype*, and inside it one may find topology
files for GROMACS and CNS/XPLOR.

To get help and more information, type:

```bash
../acpype.py -h
```

##### To Install

At folder acpype, type:

```bash
  ln -s $PWD/acpype.py /usr/local/bin/acpype
```

And re-login or start another shell session.

If via `conda` or `pip`, `acpype` should show in your `$PATH`.

##### To Verify with GMX
GROMACS < v.5.0

```bash
cd FFF.acpype/
grompp -c FFF_GMX.gro -p FFF_GMX.top -f em.mdp -o em.tpr
mdrun -v -deffnm em
# And if you have VMD
vmd em.gro em.trr
```

GROMACS > v.5.0

```bash
cd FFF.acpype/
gmx grompp -c FFF_GMX.gro -p FFF_GMX.top -f em.mdp -o em.tpr
gmx mdrun -v -deffnm em
# And if you have VMD
vmd em.gro em.trr
```

##### For MD, do:
GROMACS < v.5.0

```bash
grompp -c em.gro -p FFF_GMX.top -f md.mdp -o md.tpr
mdrun -v -deffnm md
vmd md.gro md.trr
```

GROMACS > v.5.0

```bash
gmx grompp -c em.gro -p FFF_GMX.top -f md.mdp -o md.tpr
gmx mdrun -v -deffnm md
vmd md.gro md.trr
```

###### With openmpi, for a dual core
GROMACS < v.5.0

```bash
grompp -c FFF_GMX.gro -p FFF_GMX.top -f em.mdp -o em.tpr
om-mpirun -n 2 mdrun_mpi -v -deffnm em
grompp -c em.gro -p FFF_GMX.top -f md.mdp -o md.tpr
om-mpirun -n 2 mdrun_mpi -v -deffnm md
vmd md.gro md.trr
```

GROMACS > v.5.0

```bash
gmx grompp -c FFF_GMX.gro -p FFF_GMX.top -f em.mdp -o em.tpr
gmx mdrun -ntmpi 2 -v -deffnm em
```

#### To Emulate `amb2gmx.pl`

For any given *prmtop* and *inpcrd* files (outputs from AMBER LEaP), type:

```bash
acpype -p FFF_AC.prmtop -x FFF_AC.inpcrd
```

The output files `FFF_GMX.gro` and `FFF_GMX.top` will be generated at the same
folder of the input files.

#### To Verify with CNS/XPLOR
At folder *FFF.acpype*, type:

```bash
cns < FFF_CNS.inp
```

#### To Verify with NAMD

  * see [TutorialNAMD]
