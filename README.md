# Test-Fleet Optimization Using Machine Learning
## Appendix 
### This appendix contains and briefly describes all the variables used in our work, the equations and the constraints used in the MILP model. The parameters, variables, and the final MILP model are given in the following tables


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

| Variable Symbols          | Variables                                       |
|---------------------------|-------------------------------------------------|
| $s_{jo}$                  | Start time of $O_{jo}$                         |
| $z_{jo}^{mk}$             | Indicates if $O_{jo}$ is processed on machine $m$ and with configuration $k$ |
| $y_{j^{'}o^{'}jo}^{m}$    | Indicates if $O_{jo}$ followed $O_{j^{'}o^{'}}$ on machine $m$ |
| $\bar{y}_{jo}^{m}$        | Indicates if $O_{jo}$ is the first operation on machine $m$ |
| $\hat{y}_{jo}^{m}$        | Indicates if $O_{jo}$ is the last operation on machine $m$ |
| $T_j$                     | Tardiness of job $j$                            |
| $C_j$                     | Completion time of job $j$                      |
| $PT_{jo}$                 | Processing time of $O_{jo}$                     |
| $RT_{jo}$                 | The setup time taken for $O_{jo}$               |

### The final MILP model with its constraints can be summarized as follows:
### We want to minimize the total tardiness 

$$
\sum_{j \in J} T_j
$$
This minimization is constrained by:

$$
T_j \geq 0 \quad \text{(1)}
$$
