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

The final MILP model with its constraints can be summarized as follows:

minimize   \(\sum_{j\in J} T_j\)  

subject to:

- \(T_j \geq 0\)   (con_1)
- \(T_j \geq C_j - D_j\)   (con_2)
- \(C_j \geq s_{jo} + PT_{jo}\)   \(\forall o \in H_j\)  (con_3)
- \(s_{jo} \geq s_{j^{'}o^{'}} + RT_{jo} + (y_{j^{'}o^{'}jo}^m - 1) * G\)  
  \(\forall m \in M, j, j^{'} \in J, o \in H_j, o^{'} \in H_{j^{'}}, (j, o) \neq (j^{'}, o^{'})\) (con_4)
- \(s_{jo^{'}} \geq s_{jo} + PT_{jo}\)   \(j \in J, o, o^{'} \in H_j, O_{jo} \in E_{jo^{'}}\) (con_5)
- \(s_{jo} \geq AT_j\)   (con_6)
- \(RT_{jo} \geq rt_m^{k^{'}k} * y_{j^{'}o^{'}jo}^m + (z_{jo}^{mk} + z_{j^{'}o^{'}}^{mk^{'}} - 2) * G\)  
  \(\forall m \in M, o \in H_j, o^{'} \in H_{j^{'}}, j \in J, j^{'} \in J, k \in (k_{jo} \cap K), k^{'} \in (k_{j^{'}o^{'}} \cap K), (j, o) \neq (j^{'}, o^{'})\) (con_7)
- \(RT_{jo} \geq rt_m^{ok} * \bar{y}_{jo}^{m}\)   \(\forall m \in M, j \in J, o \in H_j\) (con_8)
- \(PT_{jo} \geq p_{jo}^{mk} + (z_{jo}^{mk} - 1) * G\)  
  \(\forall j \in J, o \in H_j, k \in (k_jo \cap K)\) (con_9)
- \(\sum_{m \in M} \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk} = 1\)   \(\forall j \in J, o \in H_j\) (con_10)
- \(\sum_{j^{'} \in J} \sum_{o^{'} \in H_{j^{'}}} y_{j^{'}o^{'}jo}^m + {\bar{y}}_{jo}^m = \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk}\)  
  \(\forall m \in M, j \in J, o \in H_j\) (con_11)
- \(\sum_{j^{'} \in J} \sum_{o^{'} \in H_{j^{'}}} y_{j^{'}o^{'}jo}^m + \hat{y}_{jo}^m = \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk}\)  
  \(\forall m \in M, j \in J, o \in H_j\) (con_12)
- \(G * \sum_{j \in J} \sum_{o \in H_j} \hat{y}_{jo}^{m} \geq \sum_{j \in J} \sum_{o \in H_j} \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk}\)  
  \(\forall m \in M\) (con_13)
- \(\sum_{j \in J} \sum_{o \in H_j} \bar{y}_{jo}^m \leq 1\)   \(\forall m \in M\) (con_14)
- \(\sum_{j \in J} \sum_{o \in H_j} \hat{y}_{jo}^m \leq 1\)   \(\forall m \in M\) (con_15)
- \(G * (2 - \bar{y}_{jo}^m - \hat{y}_{jo}^m) \geq \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk} - 1\)  
  \(\forall j \in J, o \in H_j, m \in M\) (con_16)
- \(\sum_{j^{'} \in J} \sum_{o^{'} \in H_{j^{'}}} y_{j^{'}o^{'}jo}^m = \sum_{k \in (k_{jo} \cap K)} z_{jo}^{mk} - 1\)  
  \(\forall j \in J, o \in H_j, m \in M\) (con_17)
- \(\{s_{jo}, T_{j}, C_{j}, PT_{jo}, RT_{jo}\} \in R^{+}\)   (con_18)
- \(\{z_{jo}^{mk}, y_{j^{'}o^{'}jo}^{m}, \bar{y}_{jo}^{m}, \hat{y}_{jo}^{m}\} \in \{0, 1\}\)   (con_19)

