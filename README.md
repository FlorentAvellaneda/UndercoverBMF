# UndercoverBMF

Paper link : [https://www.aaai.org/AAAI22Papers/AAAI-6327.AvellanedaF.pdf](https://www.aaai.org/AAAI22Papers/AAAI-6327.AvellanedaF.pdf)

The source code is designed to be compiled and executed on GNU/Linux.


## Dependencies

### Not included
- g++ (`sudo apt install g++` on Ubuntu)
- cmake (`sudo apt install cmake` on Ubuntu)
- python3 (optional: to check the solutions with an external program)
- boost (`sudo apt install libboost-dev` on Ubuntu)

### Included
- glucose : https://www.labri.fr/perso/lsimon/glucose/
- EvalMaxSAT : https://github.com/FlorentAvellaneda/EvalMaxSAT
- CLI11 : https://github.com/CLIUtils/CLI11



## Build

```bash
cmake .
make
```


## Usage

**Usage:** `./inferbmf [OPTIONS] [SUBCOMMAND]`  

**Options:**  
  `-h,--help`            *Print this help message and exit*  
  `--solver TEXT`        *External solver cmd (internal solver by default)*  
  `--noCardGen`            *Disable Card Generation*  
  `--noBS`                *Disable Break Symmetry*  
  `--seed UINT`            *Seed (0 means random seed)*  
  `--OptiBlock`            *Activate the OptiBlock strategy*  
  `--BMF`                       *Remove the undercover constraint to infer a BMF*
  `--noFastUndercover`    *Disable the FastUndercover strategy*  
  `-k UINT`                *k (default = 1)*  
  `--optimal`            *Search for an optimal k undercover*  
  `--obs FLOAT`            *Observate data (default: 1.0)*  
  `--IG TEXT`            *Save input in a graph file*  
  `--IDAT TEXT`            *Save input in a dat file*
  `--CSV TEXT`                  *Save input in a CSV matrix file*
  `-v UINT`                *Vebosity (default: 1)*  

**Subcommands:**  
  `fromFile`                        *Input from file*  
  `fromRdm`                       *Generate an random Matrix*  

### subcommand fromFile

    **Usage:**  `./inferbmf fromFile [OPTIONS] CSV_file`  

    **Positionals:**  
      CSV_file TEXT:FILE REQUIRED CSV file  

    **Options:**  
      `-h,--help`                    *Print this help message and exit*  
      `--adj`                           *Read the input file as an adjacency list*  
      `-o TEXT`                      *output file for A and B*  
      `-O TEXT`                     *output file for A o B*  

### subcommand fromRdme

    **Usage:** `./inferbmf fromRdm [OPTIONS]`  

    **Options:**  
      `-h,--help`                   *Print this help message and exit*  
      `-k UINT`                     *Rank (default: no rank)*  
      `-d FLOAT`                  *Density (default: 0.1)*  
      `-U UINT`                    *Number of line (default: size)*  
      `-V UINT`                     *Number of column (default: size)*  
      `--size UINT`               *Size (default: 100)*

## Examples

### Optimal undercover

Search for an optimal 3-undercover for data/iris.csv and save the solution in result.A.csv and result.B.csv:

`./inferbmf -k 3 --optimal fromFile -o result data/iris.csv`

Verify the solution with an external tool:

`python3 verif.py result.A.csv result.B.csv data/iris.csv`

### FastUndercover

Search for a 50-undercover with the FastUndercover heuristic for data/iris.csv and save the solution in result.A.csv and result.B.csv:

 `./inferbmf -k 50 fromFile -o result data/iris.csv`

Verify the solution with an external tool:

`python3 verif.py result.A.csv result.B.csv data/iris.csv`

### OptiBlock

Search for a block-optimal 50-undercover initialized with <img src="https://render.githubusercontent.com/render/math?math=\bbox[white]{\{0\}^{m \times k}}"> and <img src="https://render.githubusercontent.com/render/math?math=\bbox[white]{\{0\}^{k \times n}}"> and save the solution in result.A.csv and result.B.csv:

`./inferbmf -k 50 --OptiBlock --noFastUndercover fromFile -o result data/iris.csv`

Verify the solution with an external tool:

`python3 verif.py result.A.csv result.B.csv data/iris.csv`

### OptiBlock*

Search for a block-optimal 50-undercover with a FastUndercover initialisation and save the solution in result.A.csv and result.B.csv:

`./inferbmf -k 50 --OptiBlock fromFile -o result data/iris.csv`

Verify the solution with an external tool:

`python3 verif.py result.A.csv result.B.csv data/iris.csv`


### BMF
Search for a block-optimal 2-undercover with a FastUndercover initialisation, localy optimize without the undercover constraint and save the solution in result.A.csv and result.B.csv:
`./inferbmf -k 2 --OptiBlock --BMF fromFile -o result data/breast.csv`

Verify the solution with an external tool:

`python3 verif.py result.A.csv result.B.csv data/breast.csv`


## Benchmark Result

https://raw.githubusercontent.com/FlorentAvellaneda/UndercoverBMF/main/benchmark.pdf
