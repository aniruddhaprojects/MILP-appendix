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

### This minimization is constrained by:

$$
T_j \geq 0 \quad \text{(1)}
$$

$$
T_j \geq C_j - D_j \quad \text{(2)}
$$

$$
C_j \geq s_{jo} + PT_{jo} \quad \forall o \in H_j \quad \text{(3)}
$$

$$
s_{jo} \geq s_{j^{'}o^{'}} + RT_{jo} + (y_{j^{'}o^{'}jo}^m - 1) \cdot G \quad \forall m \in M, \quad j, j^{'} \in J, \quad o \in H_j, \quad o^{'} \in H_{j^{'}}, \quad (j, o) \neq (j^{'}, o^{'}) \quad \text{(4)}
$$

$$
s_{jo^{'}} \geq s_{jo} + PT_{jo} \quad j \in J, \quad o, o^{'} \in H_j, \quad O_{jo} \in E_{jo^{'}} \quad \text{(5)}
$$

$$
s_{jo} \geq AT_j \quad \text{(6)}
$$

$$
RT_{jo} \geq rt_m^{k^{'}k} \cdot y_{j^{'}o^{'}jo}^m + (z_{jo}^{mk} + z_{j^{'}o^{'}}^{mk^{'}} - 2) \cdot G \quad \forall m \in M, \quad o \in H_j, \quad o^{'} \in H_{j^{'}}, \quad j \in J, \quad j^{'} \in J, \quad k \in (k_{jo} \cap K), \quad k^{'} \in (k_{j^{'}, o^{'}} \cap K), \quad (j, o) \neq (j^{'}, o^{'}) \quad \text{(7)}
$$

$$
RT_{jo} \geq rt_m^{ok} \cdot \bar{y}_{jo}^m \quad \forall m \in M, \quad j \in J, \quad o \in H_j \quad \text{(8)}
$$

$$
PT_{jo} \geq p_{jo}^{mk} + (z_{jo}^{mk} - 1) \cdot G \quad \forall j \in J, \quad o \in H_j, \quad k \in (k_{jo} \cap K) \quad \text{(9)}
$$

$$
\sum_{m \in M} \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk} = 1 \quad \forall j \in J, \quad o \in H_j \quad \text{(10)}
$$

$$
\sum_{j^{'} \in J} \sum_{o^{'} \in H_{j^{'}}} y_{j^{'}o^{'}jo}^m + \bar{y}}{\'}jo}^m = \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk} \quad \forall m \in M, \quad j \in J, \quad o \in H_j \quad \text{(11)}
$$

$$
\sum_{j^{'} \in J} \sum_{o^{'} \in H_{j^{'}}} y_{j^{'}o^{'}jo}^m + \hat{y}_{jo}^m = \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk} \quad \forall m \in M, \quad j \in J, \quad o \in H_j \quad \text{(12)}
$$

$$
G \cdot \sum_{j \in J} \sum_{o \in H_j} \hat{y}_{jo}^{m} \geq \sum_{j \in J} \sum_{o \in H_j} \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk} \quad \forall m \in M \quad \text{(13)}
$$

$$
\sum_{j \in J} \sum_{o \in H_j} \bar{y}_{jo}^m \leq 1 \quad \forall m \in M \quad \text{(14)}
$$

$$
\sum_{j \in J} \sum_{o \in H_j} \hat{y}_{jo}^m \leq 1 \quad \forall m \in M \quad \text{(15)}
$$

$$
G \cdot (2 - \bar{y}_{jo}^m - \hat{y}_{jo}^m) \geq \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk} - 1 \quad \forall j \in J, \quad o \in H_j, \quad m \in M \quad \text{(16)}
$$

$$
\sum_{j^{'} \in J} \sum_{o^{'} \in H_{j^{'}}} y_{j^{'}o^{'}jo}^m = \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk} - 1 \quad \forall j \in J, \quad o \in H_j, \quad m \in M \quad \text{(17)}
$$

$$
\{s_{jo}, T_{j}, C_{j}, PT_{jo}, RT_{jo}\} \in \mathbb{R}^{+} \quad \text{(18)}
$$

$$
\{z_{jo}^{mk}, y_{j^{'}o^{'}jo}^{m}, \bar{y}_{jo}^{m}, \hat{y}_{jo}^{m}\} \in \{0, 1\} \quad \text{(19)}
$$
