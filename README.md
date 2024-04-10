# Test-Fleet Optimization Using Machine Learning
## Appendix 
### This appendix contains and briefly describes all the variables used in our work, the equations and the constraints used in the MILP model.


| Parameter Symbols  | Parameters                                            |
|--------------------|-------------------------------------------------------|
| $J$                | Set of all jobs                                       |
| $M$                | Set of all machines                                   |
| $K$                | Set of all configurations                             |
| $H_j$              | Set of operations for job $j$                         |
| $O_{jo}$           | $oth$ operation of job $j$                            |
| $k_{jo}$           | Set of all compatible configurations for $O_jo$       |
| $D_j$              | Due date of job $j$                                   |
| $AT_{j}$           | Arrival time of job $j$                               |
| $rt_m^{k^{'}k}$    | Setup time to change configuration from $k′$ to $k$   |
| $rt_m^{ok}$        | Initial setup time for configuration $k$ on machine $m$|
| $p_{jo}^{mk}$      | Processing time for $O_{jo}$ on machine with $m$ with configuration $k$|
| $G$                | A large Positive Constant                             |
