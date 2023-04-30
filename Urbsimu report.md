# Urban Simulation Report
## Introduction (Scope and Objectives)
The assessment will guide you to critically investigate the resilience of the London’s underground as a network and the methodological limitations. You will do this in two ways. In the first part, you will only take into consideration the infrastructural network, where stations are connected through only one link, regardless of the number of lines connecting the stations. In the second part, you will consider the commuting flows, and discuss the impact of the analysis on the number of people moving from one part of the city to another. Then, you will recompute the flows using spatial interaction models according to different scenarios described below and discuss the vulnerability of the network under these new scenarios.

## Part 1: London’s underground resilience
### I. Topological network

This section focuses on assessing the resilience of the London Underground network as an undirected graph without considering the impact of traffic on the network. The potential impact of each station on the resilience of the London Underground is observed through the ranking of stations on the network obtained by degree centrality, betweeness centrality and closeness centrality. The impact was then quantified by defining 'Connected components' and 'Largest component size'. In addition, the role of stations in the network structure and their impact on the resilience of the network as a whole is better quantified and observed through different removal strategies.

Figure 1 illustrates the spatial distribution of the London Tube network. As this study is concerned with connectivity and interaction within the network, the maps in this study are schematic and do not show accurate spatial information such as scale etc.

![image-20230429161557294](https://p.ipic.vip/pxxj7y.png)

<center>Figure 1. Spatial distribution of London tube network</center>

#### I.1. Centrality measures

In this section, 3 centrality measures are used to evaluate the importance of nodes in the London Tube Network:

* **Degree Centrality:** It measures the number of direct neighboring nodes of a node. The formula is:
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

  This measure reflects the connectivity of a station between different areas. The higher the betweenness centrality, the greater the "bridging" role of the node in the network，which means the more important a station is for the network's connectivity, and the more crucial for connecting different parts of the network, as many passengers' journeys pass through that station.
  
* **Closeness Centrality:** It measures the reciprocal of the average shortest path length from a node to all other nodes. The formula is: 
  $$
   C_C(v) = \frac{n-1}{\sum_{u=1}^{n-1}d(u,v)}
  $$
   where $d(u,v)$ is the shortest path length between nodes $u$ and $v$.
  
   This measures the inverse of the average shortest path distance between a station and all other stations in the network. The higher the closeness centrality the station is, the more central a station is in the network,  meaning that it can reach other stations more quickly on average, which could be important for passengers' travel time.
  

The 'Networkx' library was used to calculate the ranking of the top 10 stations in the London Underground network at 3 different centrality levels, as shown in Table 1, the code and comments for which are included in the appendix at the end of this report.

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

According to the table 1, as for degree centrality, Stratford, Bank and Monument, and Baker Street are the top three stations with the highest degree centrality, suggesting that they are the busiest stations in the network. As for betweenness centrality, Stratford, Bank and Monument, and King's Cross St. Pancras are the top three stations with the highest betweenness centrality, indicating that they are essential for connecting different parts of the network. As for closeness entrality, Green Park, Bank and Monument, and King's Cross St. Pancras are the top three stations with the highest closeness centrality, suggesting that they are the most central stations in the network.

![image-20230429180128980](https://p.ipic.vip/iyo3x4.png)

<center>Figure 2. Spatial distribution of the top 10 stations for the 3 centrality measures</center>

Overall, the stations with the greatest potential impact on the resilience of the Tube network are concentrated in central London, based on the spatial distribution in Figure 2. which matches the actual situation in London. The three centrality measures provide different perspectives on the importance of stations in the London Tube Network. A station that has a high degree centrality may not necessarily be the most central station in the network, and a station with a high betweenness centrality may not necessarily have the highest traffic volume. Understanding the different measures and their implications can help with the network's management and improvement.

#### I.2. Impact measure

To find global impact measures referring to the any and whole network to evaluate the resilience by showing how the network structure is affected when nodes are removed, 2 methods were introduced at this stage:

* **The number of connected components in the network (NCC)**: This measure calculates the number of connected components in a network. A connected component is a subgraph in which every pair of nodes has a path connecting them. This measure is useful to assess how a network breaks down into smaller, isolated parts when nodes are removed. It shows how the connectivity of the network is affected by node removal.
* **The size of the largest connected component in the network (LCS)**: This measure calculates the size of the largest connected component in the network. It helps assess the robustness of the network by showing the largest remaining connected part of the network after node removals. A larger size of the largest connected component indicates better resilience against node removals. 

To illustrate these 2 measures in any general network, here is an example as shown in figure 3:

![image-20230429192413163](https://p.ipic.vip/c75ccu.png)

<center>Figure 3. Example Network for NCC and LCS</center>

In summary, both measures could be used to evaluate the resilience of any network by examining how the number and size of connected components change after node removals. They are based on the concept of connected components, which is a general network property applicable to any graph like the London Tube Network.

#### I.3. Node removal

To assess the impact of each significant station on the overall network. The changes in the London Tube network were observed through the NCC and LCS by removing 10 stations through two different removal strategies, the Non-sequential(NS) removal based on the rankings in Table 1 and Sequential removal(Seq).

* **Non-sequential(NS) removal:** remove 1 node at a time following the rank in the table 1. After each removal, evaluate the impact of the removal using NCC and LCS measures, and proceed until  10 nodes.
* **Sequential(Seq) removal:** remove the highest ranked node and evaluate the impact using the 2 measures.

The results of the station removal process are illustrated in Figures 4 and 5.

![impact_00](https://p.ipic.vip/gr66bd.png)

<center>Figure 4. Impact of removing 10 stations</center>

Figure 4 shows that, when the top 10 stations are removed, the Seq removal approach has a more pronounced effect on the overall network than NS, regardless of the centrality measures. The Seq strategy updates the network state with each removal of a top-ranked site, reflecting more significant changes. In contrast, the NS strategy only considers the initial network state, resulting in no clear trend in its values and making it difficult to assess the impact of individual sites on the overall network. Therefore, the Seq strategy is the focus of our attention.

![impact_01](https://p.ipic.vip/gcj345.png)

<center>Figure 5. Impact of removing 200(half) stations</center>

When half of the stations in the network are consistently removed based on their ranking, a more pronounced trend in the impact on the overall network becomes apparent. The results of the Seq strategy reveal that the most significant disruption to the network occurs during the removal of the top 40 stations, with the largest absolute slopes for LCS and NCC. An inflection point occurs after the removal of stations ranked between 50 and 150, while the connectivity of the entire London Underground network continues to be disrupted. This disruption has a devastating effect on the entire network after the removal of the top 150 stations.

During the removal process, NCC and LCS are both useful for assessing the damage. However, they provide different perspectives on network resilience. NCC is useful for understanding how fragmented the network becomes after node removals. A higher number of connected components indicates a more disconnected network, signifying reduced resilience.LCS represents the size of the largest remaining connected part of the network after node removals. A larger size of the largest connected component suggests better resilience against node removals. This measure is more informative for assessing overall network functioning, as it provides insight into how many nodes can still communicate with each other in the largest connected subgraph.

From a resilience perspective, 'betweenness centrality' is considered the best measure for reflecting the importance of a station in the functioning of the underground. This is because betweenness centrality takes into account the number of shortest paths passing through a node, which represents the station's role in the global network, rather than the station's local state, such as the busyness reflected by degree centrality and congregation expressed by closeness centrality 's mentioned before.

### II. Weighted network with flows

#### II.1. Weighted Centrality measures

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



![weighted](https://p.ipic.vip/zyiwmv.png)
<center>Figure 3. Spatial distribution of the top 10 stations for the 3 weighted centrality</center>

#### II.2. Weighted Impact measures

<center>Table 3. Top 10 nodes with highest reduction impact<br>(The percentage reduction in total traffic caused by the removal of one of the stations)</center>

| Rank | Station                  | Reduction |
| ---- | :----------------------- | --------- |
| 1    | Bank and Monument        | 7.14%     |
| 2    | Green Park               | 6.43%     |
| 3    | Waterloo                 | 5.82%     |
| 4    | King's Cross St. Pancras | 4.82%     |
| 5    | Westminster              | 4.35%     |
| 6    | Liverpool Street         | 4.25%     |
| 7    | Stratford                | 3.71%     |
| 8    | Euston                   | 3.66%     |
| 9    | Baker Street             | 3.21%     |
| 10   | Oxford Circus            | 3.05%     |

<center>Table 4. Top 10 nodes with highest adjusted centrality measures</center>

| Rank | station          | adjusted centrality |
| ---- | ---------------- | ------------------- |
| 1    | Green Park       | 65424602.45528399   |
| 2    | Westminster      | 64714105.45829721   |
| 3    | Hyde Park Corner | 61920252.26186064   |
| 4    | Victoria         | 58949981.58683483   |
| 5    | Waterloo         | 57846883.788449906  |
| 6    | Knightsbridge    | 57454015.02445731   |
| 7    | Barbican         | 56616116.30083641   |
| 8    | Oxford Circus    | 55051541.97973308   |
| 9    | Farringdon       | 54362334.84642131   |
| 10   | Pimlico          | 53629149.64118917   |

#### II.3. Node removal

<center>Table 5. The impact of removing the top 3 stations based on different ranks</center>

|     Measures      | Impact of top 3 nodes removal (based on the unweighted betweeness centrality rank, Table 1) | Impact of top 3 nodes removal (based on the reduction rank, Table 3) | Impact of top 3 nodes removal (based on the adjusted centrality rank, Table 4) |
| :---------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|        NCC        |                              3                               |                              1                               |                              1                               |
|        LCS        |                             377                              |                             398                              |                             398                              |
| Average Reduction |                            5.03%                             |                            6.46%                             |                            4.15%                             |

## Part 2: Spatial Interaction models
### III. Models and calibration
#### III.1. The spatial interaction models
#### III.2. Radiation model

### IV. Scenarios
#### IV.1. Scenario A: 50% job decreased in the Canary Wharf
#### IV.2. Scenario 2: A significant increase in the cost of transport
#### IV.3. Scenario 3: Discussion on the flow changes bewteen the two scenarios
