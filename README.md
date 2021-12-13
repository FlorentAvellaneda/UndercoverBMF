# UndercoverBMF

Paper link : http://florent.avellaneda.free.fr/dl/AAAI22.pdf
The source code is designed to be compiled and executed on GNU/Linux.


## Dependencies

### Not included
- g++
- cmake
- python3 (optional: to check the solutions with an external program)

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

**Usage:** `./build/inferbmf [OPTIONS] [SUBCOMMAND]`  

**Options:**  
  -h,--help            *Print this help message and exit*  
  --solver TEXT        *External solver cmd (internal solver by default)*  
  --noCardGen            *Disable Card Generation*  
  --noBS                *Disable Break Symmetry*  
  --seed UINT            *Seed (0 means random seed)*  
  --OptiBlock            *Activate the OptiBlock strategy*  
  --noFastUndercover    *Disable the FastUndercover strategy*  
  -k UINT                *k (default = 1)*  
  --optimal            *Search for an optimal k undercover*  
  --obs FLOAT            *Observate data (default: 1.0)*  
  --IG TEXT            *Save input in a graph file*  
  --IDAT TEXT            *Save input in a dat file*  
  -v UINT                *Vebosity (default: 1)*  

**Subcommands:**  
  fromFile                        *Input from file*  
  fromRdm                       *Generate an random Matrix*  

### subcommand fromFile

    **Usage:**  `./build/inferbmf fromFile [OPTIONS] CSV_file`  

    **Positionals:**  
      CSV_file TEXT:FILE REQUIRED CSV file  

    **Options:**  
      -h,--help                    *Print this help message and exit*  
      --adj                           *Read the input file as an adjacency list*  
      -o TEXT                      *output file for A and B*  
      -O TEXT                     *output file for A o B*  

### subcommand fromRdme

    **Usage:** `./build/inferbmf fromRdm [OPTIONS]`  

    **Options:**  
      -h,--help                   *Print this help message and exit*  
      -k UINT                     *Rank (default: no rank)*  
      -d FLOAT                  *Density (default: 0.1)*  
      -U UINT                    *Number of line (default: size)*  
      -V UINT                     *Number of column (default: size)*  
      --size UINT               *Size (default: 100)*   

## Examples

### Optimal undercover

Search for an optimal 3-undercover for data/iris.csv and save the solution in result.A.csv and result.B.csv:

`./build/inferbmf -k 3 --optimal fromFile -o result data/iris.csv`

Verify the solution with an external tool:

`python3 code/verif.py result.A.csv result.B.csv data/iris.csv`

### FastUndercover

Search for a 50-undercover with the FastUndercover heuristic for data/iris.csv and save the solution in result.A.csv and result.B.csv:

 `./build/inferbmf -k 50 fromFile -o result data/iris.csv`

Verify the solution with an external tool:

`python3 code/verif.py result.A.csv result.B.csv data/iris.csv`

### OptiBlock

Search for a block-optimal 50-undercover initialized with \{0\}^{m \times k} and \{0\}^{k \times n} and save the solution in result.A.csv and result.B.csv:

`./build/inferbmf -k 50 --OptiBlock --noFastUndercover fromFile -o result data/iris.csv`

Verify the solution with an external tool:

`python3 code/verif.py result.A.csv result.B.csv data/iris.csv`

### OptiBlock*

Search for a block-optimal 50-undercover with a FastUndercover initialisation and save the solution in result.A.csv and result.B.csv:

`./build/inferbmf -k 50 --OptiBlock fromFile -o result data/iris.csv`

Verify the solution with an external tool:

`python3 code/verif.py result.A.csv result.B.csv data/iris.csv`


