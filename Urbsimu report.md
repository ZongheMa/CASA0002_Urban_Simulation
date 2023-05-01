# Urban Simulation Assessment Report

[TOC]



## Introduction
This assessment aims to critically examine London's Underground network resilience, considering both infrastructure and commuting flows while addressing methodological limitations. In the first part, the focus is on the physical network, evaluating connectivity and structure. In the second part, the analysis shifts to commuting flows and their impact on the city's movement, using spatial interaction models to assess the network's vulnerability under various scenarios. The goal is to provide a comprehensive understanding of the network's resilience and potential consequences for its functionality and commuters. The word count for this report is 3136 excluding the references, numerical tables and data plots.

## Part 1: London’s underground resilience
### I. Topological network

This section studies London Underground's resilience as an undirected graph. Station rankings based on degree, betweeness and closeness centralities reveal potential impact. 'Connected components' and 'Largest component size' quantify impact. Removal strategies show stations' role and impact on the network's resilience.

Figure 1 illustrates the spatial distribution of the London Tube network. As this study is concerned with connectivity and interaction within the network, the maps in this study are schematic and do not show accurate spatial information such as scale etc.

![image-20230429161557294](https://p.ipic.vip/pxxj7y.png)
<center>Figure 1. Spatial distribution of London tube network</center>

#### I.1. Centrality measures

In this section, 3 centrality measures are used to evaluate the importance of nodes in the London Tube Network:

* **Degree Centrality:** It measures the number of direct neighboring nodes of a node(Freeman, 1978). The formula is:
  $$
  C_D(v) = \frac{d_v}{n-1}
  $$
  where $d_v$ is the degree of node $v$, i.e., the number of edges that connect to node $v$, and $n$ is the total number of nodes in the network. 

  This measure reflects the traffic volume of a station, as the more edges a station has, the more people are likely to pass through it, which could indicate it is a busy hub that serves multiple lines.

* **Betweenness Centrality:** It measures the number of times a node is traversed by all shortest paths in the network. The formula is: 
  $$
  C_B(v) = \sum_{s \neq v \neq t} \frac{\sigma_{st}(v)}{\sigma_{st}}
  $$
  where $\sigma_{st}$ is the total number of shortest paths between nodes $s$ and $t$, and $\sigma_{st}(v)$ is the number of shortest paths that pass through node $v$. 

  This measure reflects the connectivity of a station between different areas. The higher the betweenness centrality, the greater the "bridging" role of the node in the network, which means the more important a station is for the network's connectivity, and the more crucial for connecting different parts of the network, as many passengers' journeys pass through that station(Brandes, 2001).
  
* **Closeness Centrality:** It measures the reciprocal of the average shortest path length from a node to all other nodes(Bavelas, 1950 and Sabidussi, 1966). The formula is: 
  $$
   C_C(v) = \frac{n-1}{\sum_{u=1}^{n-1}d(u,v)}
  $$
   where $d(u,v)$ is the shortest path length between nodes $u$ and $v$.
  
   This measures the inverse of the average shortest path distance between a station and all other stations in the network(Evans and Chen, 2022). The higher the closeness centrality the station is, the more central a station is in the network,  meaning that it can reach other stations more quickly on average, which could be important for passengers' travel time.
  

<center>Table 1. Top 10 ranked stations for 3 Centrality Measurements</center>
| Rank | Degree Centrality                 | Betweenness Centrality            | Closeness Centrality              |
| ---- | --------------------------------- | --------------------------------- | --------------------------------- |
| 1    | Stratford (0.0225)                | Stratford (0.2978)                | Green Park (0.1148)               |
| 2    | Bank and Monument (0.02)          | Bank and Monument (0.2905)        | Bank and Monument (0.1136)        |
| 3    | Baker Street (0.0175)             | Liverpool Street (0.2708)         | King's Cross St. Pancras (0.1134) |
| 4    | King's Cross St. Pancras (0.0175) | King's Cross St. Pancras (0.2553) | Westminster (0.1125)              |
| 5    | Green Park (0.015)                | Waterloo (0.2439)                 | Waterloo (0.1123)                 |
| 6    | Canning Town (0.015)              | Green Park (0.2158)               | Oxford Circus (0.1112)            |
| 7    | Earl's Court (0.015)              | Euston (0.2083)                   | Bond Street (0.111)               |
| 8    | West Ham (0.015)                  | Westminster (0.2033)              | Farringdon (0.1107)               |
| 9    | Waterloo (0.015)                  | Baker Street (0.1916)             | Angel (0.1107)                    |
| 10   | Oxford Circus (0.015)             | Finchley Road (0.1651)            | Moorgate (0.1103)                 |

Based on Table 1, Stratford, Bank and Monument, and Baker Street are the top three busiest stations with the highest degree centrality. Stratford, Bank and Monument, and King's Cross St. Pancras are the top three stations that are essential for connecting different parts of the network, according to betweenness centrality. Green Park, Bank and Monument, and King's Cross St. Pancras are the top three most central stations, according to closeness centrality.

![image-20230429180128980](https://p.ipic.vip/iyo3x4.png)
<center>Figure 2. Spatial distribution of the top 10 stations for the 3 centrality measures</center>

Central London has the most stations with the greatest potential impact on the Tube network's resilience, as seen in Figure 2. The three centrality measures offer different perspectives on station importance, and understanding them can aid in network management and improvement. A station's high degree centrality doesn't necessarily mean it's the most central, and high betweenness centrality doesn't always mean high traffic volume.

#### I.2. Impact measure

To find global impact measures referring to the any and whole network to evaluate the resilience by showing how the network structure is affected when nodes are removed, 2 methods were introduced at this stage:

* **The number of connected components in the network (NCC)**: This measure calculates the number of connected components in a network(GeeksforGeeks, 2015). A connected component is a subgraph in which every pair of nodes has a path connecting them. This measure is useful to assess how a network breaks down into smaller, isolated parts when nodes are removed. It shows how the connectivity of the network is affected by node removal.
* **The size of the largest connected component in the network (LCS)**: This measure calculates the size of the largest connected component in the network(Stack Overflow, 2020). It helps assess the robustness of the network by showing the largest remaining connected part of the network after node removals. A larger size of the largest connected component indicates better resilience against node removals. 

To illustrate these 2 measures in any general network, here is an example as shown in figure 3:

![image-20230429192413163](https://p.ipic.vip/c75ccu.png)
<center>Figure 3. Example Network for NCC and LCS</center>

In summary, both measures could be used to evaluate the resilience of any network by examining how the number and size of connected components change after node removals. They are based on the concept of connected components, which is a general network property applicable to any graph like the London Tube Network.

#### I.3. Node removal

To evaluate each station's impact on the network, the London Tube network underwent two removal strategies: Non-sequential (NS) and Sequential (Seq). NS removes one node at a time based on Table 1 rankings, while Seq removes the highest-ranked node. Figures 4 and 5 depict the results.

* **Non-sequential(NS) removal:** remove 1 node at a time following the rank in the table 1. After each removal, evaluate the impact of the removal using NCC and LCS measures, and proceed until  10 nodes.
* **Sequential(Seq) removal:** remove the highest ranked node and evaluate the impact using the 2 measures.

The results of the station removal process are illustrated in Figures 4 and 5.

![impact_00](https://p.ipic.vip/gr66bd.png)
<center>Figure 4. Impact of removing 10 stations</center>

Figure 4 shows that, when the top 10 stations are removed, the Seq removal approach has a more pronounced effect on the overall network than NS, regardless of the centrality measures. The Seq strategy updates the network state with each removal of a top-ranked site, reflecting more significant changes. In contrast, the NS strategy only considers the initial network state, resulting in no clear trend in its values and making it difficult to assess the impact of individual sites on the overall network. Therefore, the Seq strategy is the focus of our attention.

![impact_01](https://p.ipic.vip/gcj345.png)
<center>Figure 5. Impact of removing 200(half) stations</center>

Removing top-ranked stations in a network can have a significant impact on the network's connectivity. The Seq strategy shows that the top 40 stations' removal has the most significant disruption, with the largest absolute slopes for LCS and NCC. After removing stations ranked between 50 and 150, there is an inflection point, and the entire network's connectivity remains disrupted. Removing the top 150 stations is devastating for the network. NCC measures network fragmentation, while LCS represents the size of the largest connected subgraph. Betweenness centrality is the best measure for assessing a station's importance in the network's functioning, as it reflects the station's role in the global network.
### II. Weighted network with flows

In this section the flow of passengers between stations is taken into account and is weighted to reflect the quantitative observation of the network.


#### II.1. Weighted Centrality measures

For the three centrality measures mentioned previously, the flow needs to be considered as a weight to be added to each measure, and the calculation of the weights varies from measure to measure.

**Weighted degree centrality measure**

By calculating the weights and combining it with equation (1), the calculation of the weighted degree centrality measure can be achieved by calculating weighted degree, then finding the maximum, and normalize(Candeloro, Savini and Conte, 2016):
$$
C_{WD}(v) = \frac{\sum_{u \in N(v)} w(u, v)}{W_{max}}
$$

where  $$C_{WD}(v)$$ is the normalized weighted degree centrality of node $$v$$. $$N(v)$$ is the set of neighbors (connected nodes) of node $$v$$, $$w(u, v)$$ is the weight (flow) of the edge between nodes $$u$$ and $$v$$. $$W_{max}$$ is the  maximum weighted degree among all nodes in the network $$G$$.

**Weighted betweenness centrality measure**
$$
C_{WB}(v) = \sum_{s,t \in V} \frac{\sigma_{st}(v)}{\sigma_{st}}
$$
where $$C_{WB}(v)$$ is the weighted betweenness centrality of node $$v$$, $$V$$ is the set of nodes in the graph. $$\sigma_{st}$$ is the number of shortest paths between nodes $$s$$ and $$t$$. $$\sigma_{st}(v)$$ is the number of shortest paths between nodes $$s$$ and $$t$$ that pass through node $$v$$. 

**Weighted Closeness Centrality**
$$
C_{WC}(v) = \frac{1}{\sum_{u \in V} d(v, u)}
$$
where $$C_{WC}(v)$$ is the weighted closeness centrality of node $$v$$. $$V$$ is the set of nodes in the network. $$d(v, u)$$ is the shortest path distance between nodes $$v$$ and $$u$$, computed using the inverted edge weights like flows.

For weighted betweenness and closeness centrality, inverted flows are used as weights to avoid congestion. The London tube network resilience study uses different weighting methods for centrality measures. Weighted degree centrality uses flows, while weighted betweenness and closeness centrality use inverted flows(networkx.org, n.d.), emphasizing station roles and relative convenience. Inverted flows are used as weights so that sites with higher flows are 'penalised' and sites with lower flows are 'rewarded' in the shortest path computations.Inverted flows reveal congestion levels, helping identify key nodes for efficient network operation(Avrachenkov et al., 2013).The top 10 ranked stations for 3 weighted centrality measures is shown in table 2.


<center>Table 2. Top 10 ranked nodes for 3 Weighted Centrality Measurements</center>
| Rank | Weighted Degree Centrality        | Weighted Betweenness Centrality | Weighted Closeness Centrality |
| ---- | --------------------------------- | ------------------------------- | ----------------------------- |
| 1    | Bank and Monument (1.0)           | Green Park (0.5492)             | Green Park (0.0001)           |
| 2    | Green Park (0.9001)               | Bank and Monument (0.5267)      | Westminster (0.0001)          |
| 3    | Waterloo (0.8151)                 | Waterloo (0.4256)               | Waterloo (0.0001)             |
| 4    | King's Cross St. Pancras (0.6749) | Westminster (0.3743)            | Bank and Monument (0.0001)    |
| 5    | Westminster (0.6096)              | Liverpool Street (0.3441)       | Oxford Circus (0.0001)        |
| 6    | Liverpool Street (0.5953)         | Stratford (0.3375)              | Liverpool Street (0.0001)     |
| 7    | Stratford (0.5201)                | Euston (0.2722)                 | Bond Street (0.0001)          |
| 8    | Euston (0.513)                    | Oxford Circus (0.2472)          | Warren Street (0.0001)        |
| 9    | Baker Street (0.4499)             | Bond Street (0.2447)            | Hyde Park Corner (0.0001)     |
| 10   | Oxford Circus (0.4277)            | Baker Street (0.2404)           | Moorgate (0.0001)             |

Comparing centrality in the London tube network, weighted measures better represent station importance by considering flows. Stratford ranks high in unweighted metrics but declines in weighted ones due to low flows. Green Park excels in weighted betweenness and closeness, effectively connecting lines and aiding other stations. Bank and Monument hold critical roles due to high flow and importance. Westminster and Liverpool Street's improved weighted rankings emphasize their significance as key nodes. Weighted metrics, focusing on actual usage and connection strength, provide more informative assessments of the London Underground network.

![weighted](https://p.ipic.vip/zyiwmv.png)
<center>Figure 6. Spatial distribution of the top 10 stations for the 3 weighted centrality</center>

Comparing the spatial distribution in figures 2 and 6 reveals that flow-weighted centrality is more realistic, accurately reflecting variations in passenger flows. The top 10 stations, located in central London near landmarks, highlight the measure's improved accuracy in assessing the London Tube network's resilience.

#### II.2. Weighted Impact measures

To assess a weighted network, station and weight changes' effects on network connectivity and vulnerability need to be considered using NCC and LSC. Two methods were proposed to gauge the London tube network's resilience: Reduction Impact based on node-associated flows and Adjusted Centrality based on PageRank score.

**Reduction Impact** is calculated as:
$$
RI = \frac{\sum_{(u,v) \in E} Flow_{(u,v)} - \sum_{(u,v) \in E'} Flow'*{(u,v)}}{\sum*{(u,v) \in E} Flow_{(u,v)}}
$$

where $RI$ is the reduction impact, $E$ is the set of edges of the network $G$, $(u, v)$ is the flow of $Flow_{(u,v)}$ denoting edge $(u, v)$. $E'$ is the set of edges of graph $G'$, $(u, v)$ is the set of edges, and $Flow'_{(u, v)}$ denotes the flow of edge $(u, v)$ in the modified network. The network $G'$ is obtained by removing the specified node from the network $G$.

The reduction impact measures the impact of removing a node on the total flow of the network. It calculates the percentage decrease in total traffic after the removal of a node from the original network. The equation captures the total flow of the original network, as well as the modified network's total flow (after node removal), and an indication of the difference between them.

The PageRank algorithm is a popular method for ranking nodes in a network based on their importance. The basic idea is that a node is more important if it has many incoming edges from other important nodes, which is also a general and global measure for all networks as calculated in equation (11):
$$
PR(u) = \frac{1-d}{N} + d \sum_{v \in M(u)} \frac{PR(v)}{L(v)}
$$
where, $PR(u)$: The PageRank value of node $u$. $d$: The damping factor. $N$: The total number of nodes in the network. $M(u)$: The set of all nodes pointing to node $u$. $PR(v)$: The PageRank value of node $v$ in the previous iteration. $L(v)$: The number of outgoing edges from node $v$. It iterates until convergence or reaching maximum iterations.

**Adjusted Centrality** combines weighted degree and PageRank:
$$
AC(u) = \frac{\sum_{v \in N(u)} w_{u,v}}{PR(u)}
$$

where, $AC(u)$: Adjusted Centrality of node $u$. $N(u)$: The set of all neighboring nodes of node $u$. $w_{u,v}$: The weight (flows) of the edge between nodes $u$ and $v$. $PR(u)$: The PageRank value of node $u$.

It represents a measure of the node's importance in the network, considering both its weighted degree and its PageRank score(Shaw, 2019). It is a useful measure for evaluating the importance of nodes in the London tube network with respect to both connectivity and flow. The results obtained from reduction impact and adjusted centrality are shown in Tables 3 and 4, where roughly the same stations are covered, although the ranks differ.

<center>Table 3. Top 10 nodes with highest reduction impact<br>(The percentage reduction in total traffic caused by the removal of one of the stations)</center>
| Rank |         Station          | Reduction impact |
| :--: | :----------------------: | :--------------: |
|  1   |    Bank and Monument     |      7.14%       |
|  2   |        Green Park        |      6.43%       |
|  3   |         Waterloo         |      5.82%       |
|  4   | King's Cross St. Pancras |      4.82%       |
|  5   |       Westminster        |      4.35%       |
|  6   |     Liverpool Street     |      4.25%       |
|  7   |        Stratford         |      3.71%       |
|  8   |          Euston          |      3.66%       |
|  9   |       Baker Street       |      3.21%       |
|  10  |      Oxford Circus       |      3.05%       |


<center>Table 4. Top 10 nodes with highest adjusted centrality measures</center>
| Rank |     station      | adjusted centrality |
| :--: | :--------------: | :-----------------: |
|  1   |    Green Park    |     65424602.45     |
|  2   |   Westminster    |     64714105.45     |
|  3   | Hyde Park Corner |     61920252.26     |
|  4   |     Victoria     |     58949981.58     |
|  5   |     Waterloo     |     57846883.78     |
|  6   |  Knightsbridge   |     57454015.02     |
|  7   |     Barbican     |     56616116.30     |
|  8   |  Oxford Circus   |     55051541.97     |
|  9   |    Farringdon    |     54362334.84     |
|  10  |     Pimlico      |     53629149.64     |

Table 3 displays the top 10 nodes with the highest reduction impact, illustrating the traffic decrease from removing a station. Bank and Monument station lead (7.14%), followed by Green Park (6.43%) and Waterloo (5.82%). Table 4 presents the top 10 nodes with the highest adjusted centrality, accounting for weighted degree and PageRank. Green Park tops the list (65,424,602.46), followed by Westminster (64,714,105.46) and Hyde Park Corner (61,920,252.26).

Both measures identify key London tube stations, with significant overlap. This suggests both measures effectively assess station importance for network resilience and connectivity. Ranking differences reveal each measure's unique perspective on critical nodes. Reduction impact examines station removal's direct effect on traffic, while adjusted centrality evaluates a node's importance via connectivity and traffic. Analyzing both measures provides a comprehensive understanding of network resilience. High values in both measures show the London tube network's vulnerability. If critical stations were impacted, significant disruptions, delays, and inconveniences could occur.

In conclusion, reduction impact and adjusted centrality offer crucial insights into London tube network nodes. Considering both measures is vital for planning network resilience, maintenance, and improvement projects, ensuring network efficiency and robustness against disruptions.

#### II.3. Node removal

<center>Table 5. The impact of removing the top 3 stations based on different ranks</center>

|          Measures           | Impact of top 3 nodes removal (based on the unweighted betweeness centrality rank, Table 1) | Impact of top 3 nodes removal (based on the reduction rank, Table 3) | Impact of top 3 nodes removal (based on the adjusted centrality rank, Table 4) |
| :-------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|             NCC             |                              3                               |                              1                               |                              1                               |
|             LCS             |                             377                              |                             398                              |                             398                              |
|        Sum reduction        |                            15.10%                            |                            19.39%                            |                            12.45%                            |
| Average adjusted centrality |                         40910474.06                          |                         58061888.19                          |                         64019653.39                          |


The analysis shows that closing top-ranked stations (Bank and Monument, Green Park, and Waterloo) would significantly impact passengers, with a 19.39% reduction in total flows, while maintaining network connectivity. These stations' high adjusted centrality values also indicate their importance for connectivity and flow.

These high-impact stations can be categorized as:

- **Central Stations**: Located in central London, they serve as transit hubs connecting multiple lines. Their closure could cause congestion and delays.
- **Tourist Attractions**: Near popular tourist spots, these stations experience high passenger traffic. Closures could affect tourists and local businesses.
- **Interchange Stations**: Where multiple lines converge, they distribute passenger flow. Closing them could increase congestion at other interchange points.
- **Commuter Hubs**: Connecting the Tube to other transport services, their closure could disrupt commuters.

In conclusion, high-impact stations are crucial to the London Tube network due to their central locations, proximity to tourist attractions, interchange roles, and transport connections. Disruptions or closures could significantly impact passengers and the network's efficiency, emphasizing the importance of maintaining the resilience of key stations.

## Part 2: Spatial Interaction models

### III. Models and calibration
#### III.1. The spatial interaction models

Table 6 provides an overview of some spatial interaction models covered, including their formulations and interpretation of parameters, and offers common application scenarios(O’Kelly, 2009):

<center>Table 6. Overview of the spatial interaction models  </center>

|          Model          |  Constraints   |                           Formula                            |  Cost Function  | Parameter Explanation                                        | Key Features                                                 | Applicable Scenarios                                |
| :---------------------: | :------------: | :----------------------------------------------------------: | :-------------: | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------------- |
|   Unconstrained Model   |      None      | $T_{ij} = k * \frac{A_{i}^{\alpha} * B_{j}^{\beta}}{D_{ij}^{\gamma}}$ | $D_{ij}^\gamma$ | $k$: constant, $α$, $β$, $γ$: parameters to be estimated.    | Interaction depends on attributes ($A_i$, $B_j$) and distance; no flow constraints. | Migration, trade, transportation.                   |
|   Singly Constrained    | Origin or Dest |             $T_{ij} = A_{i} * B_{j} * F(D_{ij})$             |   $F(D_{ij})$   | $A_i$, $B_j$: attributes, $F(D_{ij})$: distance function.    | Interaction depends on distance; constraints on either origin or destination but not both. | Trip distribution, resource allocation.             |
|   Doubly Constrained    |      Both      | $T_{ij} = \frac{A_{i} * B_{j} * F(D_{ij})}{\sum\limits_{k} B_{k} * F(D_{ik})}$ |   $F(D_{ij})$   | $A_i$, $B_j$: attributes, $F(D_{ij})$: distance function, ${\sum\limits_{k}}$: summation over all spatial units $k$. | Interaction depends on distance; constraints on both origin and destination. | Freight distribution, transportation planning.      |
|      Network Model      |     Varies     |                              -                               |     Varies      | -                                                            | Represents spatial interactions as a graph; cost based on distance, time, or other impedance. | Transportation planning, traffic flow analysis.     |
| Cellular Automata Model |       -        |               $$ S(t+1) = R(S(t), N(S(t))) $$                |        -        | $S(t)$: state of a cell,$ N(S(t))$: neighborhood, $R$: transition rule. | Discrete, dynamic system; state evolves based on predefined rules and neighborhood states. | Land use change, urban growth, ecosystem dynamics.  |
|    Agent-Based Model    |       -        |                              -                               |        -        | -                                                            | Consists of autonomous, interacting agents; model described by algorithm or equations. | Pedestrian movement, social interaction, epidemics. |

In the gravity model(Anderson, 2011), $α$, $β$, and $γ$ define interactions between spatial units $i$ and $j$.

- $α$: Affects origin unit $i$'s attribute $A_i$ (e.g., population) on interaction. Higher $α$ implies greater interaction with increasing $A_i$.
- $β$: Affects destination unit $j$'s attribute $B_j$ (e.g., demand) on interaction. Higher $β$ implies greater interaction with increasing $B_j$.
- $γ$: Affects distance $D_{ij}$ between units on interaction. Higher $γ$ means faster interaction decrease as distance grows, indicating distance decay effect.

Parameters are estimated through calibration using empirical data, aiming to find best-fitting values with methods like OLS regression or MLE.

#### III.2. Modelling and calibration

The gravity model is ideal for analyzing the London Tube network due to its:

1. **Intuitive Interpretation:** The model captures the effects of population, jobs, and distance on travel demand, making it a logical choice.
2. **Key Variables Inclusion:** The gravity model integrates population, jobs, and distance, effectively representing factors influencing travel patterns.
3. **Widespread Use and Reliability:** As a popular and tested model, it is reliable for transportation planning and analysis.
4. **Calibration and Validation:** Parameters can be calibrated with observed data, allowing for improved understanding and predictive accuracy.
5. **Flexibility and Adaptability:** The model can incorporate additional factors, enabling a comprehensive analysis of the London Tube network.

In summary, the gravity model's ability to integrate key variables, intuitive interpretation, widespread use, and adaptability make it suitable for modeling the London Tube network.The key steps of modelling fitting gravity model to London tube network taking into account the job, population and flows are as follows:

1. **Pre-process:** Import the necessary libraries, normalizing the data and eliminating flows with zero distance.
2. **Define cost functions:** Four different cost functions are defined: linear, exponential, power, and logarithmic. These functions will be used to model the effect of distance on the spatial interaction.
3. **Define the gravity model function:** The gravity model function takes distance, population, jobs, and cost function as inputs to calculate the interaction between spatial units.
4. **Define the objective function for cross-validation:** Defined objective function calculates the average of mean squared errors for both train and test sets, which will be minimized during cross-validation.
5. **Perform cross-validation:** Using 10-fold cross-validation, the gravity model is fit to the data using each of the four cost functions. The best cost function is selected based on the lowest average error across the 10 folds.
6. **Select the best set of parameters:** The best set of parameters (a, b, and c) is chosen by the cross-validation.
  * **a**: controls the effect of population on the flows;
  * **b**: controls the effect of jobs on the flows;
  * **c**: controls the impact of distance on the flows through the cost function.
7. **Analysis and visualization:** Print the best parameters set and visulize the training process and results.

This modelling approach is justified as it:

1. Incorporates varied cost functions for distance, enabling flexible distance decay effects.
2. Utilizes cross-validation, minimizing overfitting and offering reliable performance estimates.
3. Selects the optimal cost function and parameters via data, likely improving predictions.
4. Interprets parameters, aiding comprehension of relationships in the London Tube network.

![output_00](https://p.ipic.vip/zva4yy.png)

<center>Figure 7. The results of the gravity model</center>
According to the results of the gravity model as shown in the figure 7, conclusions were drown as following:

1. As the best cost function, *logarithmic cost function* implies that the interaction decreases more rapidly at shorter distances and levels off as the distance increases. This cost function fits better than the linear, exponential, or power cost functions, demonstrating that the logarithmic decay captures the underlying distance decay effect more accurately for the London Tube network.
2. The best error obtained is 3.1728e-05, which is a small value. This low error suggests that the model fits the data well and is capable of explaining the variability in the flows based on the given parameters.
3. Best parameters: **a = 1.156** and **b = 1.158** indicate the effect of population and jobs on the flows is slightly more than linear. As population or job opportunities increases, the flows also increase, but at a slightly accelerated rate. In this case, **c = 0.131** indicates a moderate decay rate in the interaction as the distance increases.

These results provide insights into the relationships between population, jobs, and distance on the flows in the London Tube network. The gravity model with the logarithmic cost function and the best parameters provide a good fit to the data, suggesting that it can effectively capture the underlying patterns of interaction in the network.

### IV. Scenarios
#### IV.1. Scenario A: 50% job decreased in the Canary Wharf

In this simulation, the impact of a 50% decrease in jobs at Canary Wharf after Brexit was assessed  by first identifying the station's index in the dataset, creating a modified dataset with reduced jobs at Canary Wharf, and re-scaling the updated jobs column. Using the best parameters and the best cost function, the new flows was computed  for Scenario A. To ensure commuter conservation, normalize the new flows, adjusting them proportionally so that the total number of commuters remains the same as the original. After updating the dataset with the new normalized flows, the effects of the job decrease on the flows to and from Canary Wharf station could be observed.

![output_01](https://p.ipic.vip/9aq0i7.png)
<center>Figure 8. Spatial distribution of flow change results for Scenario A</center>

The gravity model accurately predicts the impact of Brexit on London's tube system. The model adjusts the number of commuters to remain constant by normalizing total flows and recalculating new flows. The model predicts a 40% decrease in the number of commuters traveling to Canary Wharf due to job loss, while there is a 22% increase in commuters leaving. This shift in commuter flows to other stations shows how the model can simulate changes in job distribution, as seen in figure 8.

#### IV.2. Scenario B: A significant increase in the cost of transport

Scenario B assesses the impact of an increase in transport cost on flow distribution by choosing two new values for the parameter $c$ in the cost function, namely 1.5 and 10 times the best parameter. The gravity model is used to compute the flow distribution with updated $c$ values, while $a$ and $b$ parameters remain constant. Normalization factors are then calculated based on the original and new flows, and the new flows are adjusted accordingly. The updated dataset shows how an increase in transport cost affects commuting patterns and can be used to analyze the relationship between transport cost and flow distribution.

![output_02](https://p.ipic.vip/qe3tsh.png)
<center>Figure 9. Spatial distribution of flow change results for Scenario B</center>

Increasing commuting resistance (inflating c) can alter the distribution of London Underground flows, as shown in Figure 9. Commuters tend to choose nearby destinations to save money, leading to concentrated flows over short distances and reduced flows over longer distances. Urban areas with many jobs or transportation options may not feel the effects as strongly, while suburban and rural areas with limited options and longer distances between homes and work may be more affected. Colors in the figure indicate radial variations in both cases.

#### IV.3. Discussion on the flow changes bewteen the two scenarios

<center>Table 7. Top 10 Lines with the most significant flow changes in different scenarios</center>
| Rank | Scenario A                           | Scenario B1 | Scenario B2 |
| ---- | ------------------------------------ | ----------- | ----------- |
| 1    | Waterloo - Liverpool Street          |Waterloo - Liverpool Street| Waterloo - Liverpool Street |
| 2    | London Bridge - Bank and Monument | London Bridge - Bank and Monument | London Bridge - Bank and Monument |
| 3    | Liverpool Street - Bank and Monument | Liverpool Street - Bank and Monument | Liverpool Street - Bank and Monument |
| 4    | Stratford - Bank and Monument | Stratford - Bank and Monument | Stratford - Bank and Monument |
| 5    | Waterloo - Canary Wharf | Victoria - Bank and Monument | Victoria - Bank and Monument |
| 6    | London Bridge - Canary Wharf | Waterloo - Victoria | Waterloo - Victoria |
| 7    | Victoria - Bank and Monument | London Bridge - Liverpool Street | London Bridge - Liverpool Street |
| 8    | Waterloo - Victoria | Waterloo - Oxford Circus | Waterloo - Oxford Circus |
| 9    | London Bridge - Liverpool Street | Waterloo - Bank and Monument | Bank and Monument - Liverpool Street |
| 10 | Waterloo - Oxford Circus | Bank and Monument - Liverpool Street | Canada Water - Bank and Monument |

To discuss flow changes in three different scenarios, we examine the top 10 lines with the largest changes in flow in each scenario. Scenario A has a 50% decrease in jobs at Canary Wharf, affecting flows between major stations such as Waterloo, London Bridge, Liverpool Street, and Bank and Monument, including the flow between Waterloo and Canary Wharf. Scenario B consists of two sub-scenarios with higher transport costs, causing flow changes between major stations such as Waterloo, London Bridge, Liverpool Street, Bank and Monument, and Victoria. Higher transport costs consistently impact flows across the network. Compared to Scenario A, Scenario B has a broader effect on the network, causing changes in flows across several key lines, which is attributed to the fact that increased transport costs impact commuters' decisions across the entire network, leading to more significant redistribution of flows. Based on the analysis, Scenario B has a greater impact on flow redistribution in terms of increased transport costs compared to Scenario A.

## References

Anderson, J.E. (2011). The Gravity Model. *Annual Review of Economics*, 3(1), pp.133–160. doi:https://doi.org/10.1146/annurev-economics-111809-125114.

Anon, (n.d.). *Letian gis*. [online] Available at: https://letiangis.com/london-underground-network-study/ [Accessed 1 May 2023].

Avrachenkov, K., Litvak, N., Medyanikov, V. and Sokol, M. (2013). *Alpha current flow betweenness centrality*. [online] Available at: https://core.ac.uk/download/pdf/19487434.pdf [Accessed 1 May 2023].

Bavelas, A. (1950). Communication Patterns in Task‐Oriented Groups. *The Journal of the Acoustical Society of America*, 22(6), pp.725–730. doi:https://doi.org/10.1121/1.1906679.

Brandes, U. (2001). A faster algorithm for betweenness centrality*. *The Journal of Mathematical Sociology*, 25(2), pp.163–177. doi:https://doi.org/10.1080/0022250x.2001.9990249.

Candeloro, L., Savini, L. and Conte, A. (2016). A New Weighted Degree Centrality Measure: The Application in an Animal Disease Epidemic. *PLOS ONE*, 11(11), p.e0165781. doi:https://doi.org/10.1371/journal.pone.0165781.

Evans, T.S. and Chen, B. (2022). Linking the network centrality measures closeness and degree. *Communications Physics*, 5(1). doi:https://doi.org/10.1038/s42005-022-00949-5.

Freeman, L.C. (1978). Centrality in social networks conceptual clarification. *Social Networks*, 1(3), pp.215–239. doi:https://doi.org/10.1016/0378-8733(78)90021-7.

GeeksforGeeks. (2015). *Connected Components in an undirected graph*. [online] Available at: https://www.geeksforgeeks.org/connected-components-in-an-undirected-graph/.

networkx.org. (n.d.). *betweenness_centrality — NetworkX 1.10 documentation*. [online] Available at: https://networkx.org/documentation/networkx-1.10/reference/generated/networkx.algorithms.centrality.betweenness_centrality.html.

O’Kelly, M.E. (2009). Spatial Interaction Models. *Elsevier eBooks*, 3, pp.365–368. doi:https://doi.org/10.1016/b978-008044910-4.00529-0.

Sabidussi, G. (1966). The centrality index of a graph. *Psychometrika*, 31(4), pp.581–603. doi:https://doi.org/10.1007/bf02289527.

Shaw, A. (2019). *How a Page Rank is calculated in Gephi*. [online] Strategic Planet. Available at: https://www.strategic-planet.com/2019/12/how-a-page-rank-is-calculated-in-gephi/#:~:text=The%20Page%20Rank%20concept%20is.

Stack Overflow. (2020). *language agnostic - Google interview algorithm puzzle: expected size of the largest connected component in a random simple graph (N nodes, N edges)?* [online] Available at: https://stackoverflow.com/questions/28527891/google-interview-algorithm-puzzle-expected-size-of-the-largest-connected-compon [Accessed 1 May 2023].

## Code and comments



