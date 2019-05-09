# HemiplasyTool

## Authors:
Matt Gibson (gibsomat@indiana.edu)  
Mark Hibbins (mhibbins@indiana.edu)

## Dependencies:
* ms  
* seq-gen  
* biopython
* numpy


## Usage
```
usage: hemiplasytool.py [-h] [-v] [-n] [-x] [-p] [-g] [-o] splits traits tree

Calculate the probability that convergent trait patterns are due to hemiplasy

positional arguments:
  splits              Split times file, ordered from oldest to newest. In
                      units of 4N generations.
  traits              Traits file
  tree                Species topology in Newick format on one line.

optional arguments:
  -h, --help          show this help message and exit
  -v, --verbose       Enable debugging messages to be displayed
  -n , --replicates   Number of replicates per batch
  -x , --batches      Number of batches
  -p , --mspath       Path to ms
  -g , --seqgenpath   Path to seq-gen
  -o , --outputdir    Output directory
```

## Input file

The input file has three sections:  split times, traits, and species tree. They must be specified in this order and delimited by a '#'. See below for descriptions of each section

```
#Split times
6   2   1
3   3   2
1.5 5   3
1.25    6   5
1   4   3

#Traits
1   0 
2   1
3   0
4   1
5   0
6   1

#Species tree
(1,(2,((6,5),(4,3))));

```

### Split times

The split times describe the order of subpopulation splits to `ms`. Each line specifies the timing (in 4N generations), source population, and destination population (backwards in time). Splits should be ordered oldest to newest. Entries should be delimited by spaces or tabs


### Traits

The traits section describes the observed species trait pattern. Each line specifies the taxa ID (must correspond to those coded in the split times file), the binary trait value, and the timing of sampling (in 4N generations relative to the longest branch). These can be specified in any order


### Species tree

The species tree in Newick format. Again, taxa IDs must correspond to those in the split times and traits sections.


## Example:
```
python hemiplasytool/hemiplasytool.py -v -n 1000000 -p ~/bin/ms -g ~/bin/seq-gen -x 1 ./input_test.txt
```

### Output:
```
Of the replicates that follow species site pattern:
12 were discordant
11 were concordant


On concordant trees:
# Mutations	# Trees
2		1
3		10

On discordant trees:
# Mutations	# Trees
2		2
3		9
4		1
DEBUG:root:Plotting...

Time elapsed: 4.960004091262817 seconds
```

![Mutation distribution](mutation_dist.png)