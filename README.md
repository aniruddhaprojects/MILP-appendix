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

[![\\ \mathbf{minimize} \hspace{1em} \sum_{j\in J} T_j \label{obj} \\ ]

subject to:

- T_j ≥ 0   (con_1)
- T_j ≥ C_j - D_j   (con_2)
- C_j ≥ s_{jo} + PT_{jo}   ∀o ∈ H_j  (con_3)
- s_{jo} ≥ s_{j^{'o^{'}}} + RT_{jo} + (y_{j^{'}o^{'}jo}^m - 1) * G  
  ∀m ∈ M, j, j^{'} ∈ J, o ∈ H_j, o^{'} ∈ H_{j^{'}}, (j, o) ≠ (j^{'}, o^{'}) (con_4)
- s_{jo^{'}} ≥ s_{jo} + PT_{jo}  
  j ∈ J, o, o^{'} ∈ H_j, O_{jo} ∈ E_{jo^{'}} (con_5)
- s_{jo} ≥ AT_j   (con_6)
- RT_{jo} ≥ rt_m^{k^{'k}} * y_{j^{'}o^{'}jo}^m + (z_{jo}^{mk} + z_{j^{'}o^{'}}^{mk^{'}} - 2) * G 
  ∀m ∈ M, o ∈ H_j, o^{'} ∈ H_{j^{'}}, j ∈ J, j^{'} ∈ J, 
  k ∈ (k_{jo} ∩ K), k^{'} ∈ (k_{j^{'}, o^{'}} ∩ K), (j, o) ≠ (j^{'}, o^{'}) (con_7)
- RT_{jo} ≥ rt_m^{ok} * \bar{y}_{jo}^m   ∀m ∈ M, j ∈ J, o ∈ H_j (con_8)
- PT_{jo} ≥ p_{jo}^{mk} + (z_{jo}^{mk} - 1) * G   
  ∀j ∈ J, o ∈ H_j, k ∈ (k_{jo} ∩ K) (con_9)
- ∑_{m ∈ M} ∑_{k ∈ (k_{jo} ∩ K)} z_{jo}^{mk} = 1   
  ∀j ∈ J, o ∈ H_j (con_10)
- ∑_{j^{'} ∈ J} ∑_{o^{'} ∈ H_{j^{'}}} y_{j^{'}o^{'}jo}^m + \bar{y}_{jo}^m = ∑_{k ∈ (k_{jo} ∩ K)} z_{jo}^{mk} 
  ∀m ∈ M, j ∈ J, o ∈ H_j (con_11)
- ∑_{j^{'} ∈ J} ∑_{o^{'} ∈ H_{j^{'}}} y_{j^{'}o^{'}jo}^m + \hat{y}_{jo}^m = ∑_{k ∈ (k_{jo} ∩ K)} z_{jo}^{mk} 
  ∀m ∈ M, j ∈ J, o ∈ H_j (con_12)
- G * ∑_{j ∈ J} ∑_{o ∈ H_j} \hat{y}_{jo}^{m} ≥ ∑_{j ∈ J} ∑_{o ∈ H_j} ∑_{k ∈ (k_{jo} ∩ K)} z_{jo}^{mk}   
  ∀m ∈ M (con_13)
- ∑_{j ∈ J} ∑_{o ∈ H_j} \bar{y}_{jo}^m ≤ 1   ∀m ∈ M (con_14)
- ∑_{j ∈ J} ∑_{o ∈ H_j} \hat{y}_{jo}^m ≤ 1   ∀m ∈ M (con_15)
- G * (2 - \bar{y}_{jo}^m - \hat{y}_{jo}^m) ≥ ∑_{k ∈ (k_{jo} ∩ K)} z_{jo}^{mk} - 1   
  ∀j ∈ J, o ∈ H_j, m ∈ M (con_16)
- ∑_{j^{'} ∈ J} ∑_{o^{'} ∈ H_{j^{'}}} y_{j^{'}o^{'}jo}^m = ∑_{k ∈ (k_{jo} ∩ K)} z_{jo}^{mk} - 1   
  ∀j ∈ J, o ∈ H_j, m ∈ M (con_17)
- \{s_{jo}, T_{j}, C_{j}, PT_{jo}, RT_{jo}\} ∈ R^{+}   (con_18)
- \{z_{jo}^{mk}, y_{j^{'}o^{'}jo}^{m}, \bar{y}_{jo}^{m}, \hat{y}_{jo}^{m}\} ∈ \{0, 1\}   (con_19)
