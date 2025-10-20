# Datasets v2

> Note 1: This dataset has been generated with [BNNetSimulator](https://github.com/BNN-UPC/BNNetSimulator)

> Note 2: It is highly recommended to use the API provided [[here](https://github.com/knowledgedefinednetworking/datanetAPI/tree/challenge2020)] to easily read and process samples from the dataset. However, if you prefer to process directly the raw data, you can find the description of the dataset format below.

> These datasets were used in the [Graph Neural Networking challenge 2020](https://bnn.upc.edu/challenge/gnnet2020/).

### Dataset description

This repository contains datasets with simulation results of delay, jitter and packet loss for four different network topologies: NSFNET (14 nodes), GBN (17 nodes), REDIRIS (19 nodes) and GEANT2 (24 nodes).

<table style="text-align:center;">
  <tr>
    <td><img src="/assets/nsfnet_topology.png" width="400" alt></td>
    <td><img src="/assets/gbn_topology.png" width="400" alt></td>
  </tr>
  <tr>
    <td>NSFNET Topology</td>
    <td>GBN Topology</td>
  </tr>
   <tr>
     <td><img src="/assets/rediris_topology.png" width="400" alt></td>
     <td><img src="/assets/geant2_topology.png" width="400" alt></td>
  </tr>
  <tr>
     <td>REDIRIS Topology</td>
     <td>GEANT2 Topology</td>
  </tr>
 </table>

We provide three datasets for training, validation, and test. The training dataset contains samples simulated in the NSFNET and GEANT2 network topologies, while the validation dataset contains samples simulated in GBN. The test dataset contains samples simulated in the RedIRIS network topology. We initially released a test dataset that does not contain labels of per-flow performance metrics (e.g., delay), as it was used to evaluate participants' solutions during the challenge. After the end of the competition, we released a new version of this latter dataset including these labels. Note that the test dataset contains samples with similar distributions to those included in the validation dataset.

These datasets have their focus on adding queue scheduling policies that may be complex to model. To this end, we simulate scenarios where devices are configured with different scheduling policies. The following policies policies are used: Strict Priority (SP), where packets in queues with more priority are transmitted first, Weighted Fair Queueing (WFQ), and Deficit Round Robin (DRR).

For all the queue scheduling policies implemented, we consider always three queues with different priorities on nodes. Thus, each sample of the dataset includes a traffic matrix where flows may have 3 different Type of Services (ToS=[0,1,2]) respectively associated to one of the three queues on nodes (e.g., ToS=0 is associated to the first queue of nodes). At the beginning of each simulation, a ToS is associated to the flows generated in a source-destination path. All the packets of the path will have the same ToS. Packets are generated on each flow following a Poisson distribution. To this end, we use an exponential time distribution (TimeDist.EXPONENTIAL_T) to model inter-packet arrival times. Likewise, we use a binomial distribution (SizeDist.BINOMIAL_S) to model packet size. The maximum bitrate that paths may have (maxAvgLambda) is selected randomly for each simulation, between 400 and 2000 bits per time unit. Note that packet delay is limited to 20 time units for all the simulation scenarios.

### Queue scheduling

The datasets contain samples randomly ordered from 4 different queue scheduling scenarios and including the same proportion of samples of each scenario (i.e., 25% each scenario). These 4 scenarios are described below:

**Scenario 1:** In this scenario all nodes are configured with a WFQ policy and the weights assigned to each queue are 60% for flows with ToS=0, 30% for ToS=1 and 10% for ToS=2. ToS are assigned to flows with the following probability: 10% for ToS=0, 30% for ToS=1 and 60% for ToS=2.

**Scenario 2:** Like in scenario 1, all nodes of the topology are configured with a WFQ policy, but, in this case, we define 5 different profiles including different queue weights. Each node is configured randomly with one of these profiles. A total of 100 scheduling configurations (i.e., nodes + weight profiles) are generated with this criterion for each topology, and each simulation selects randomly one of these configurations.

|  **Weight profile**| **ToS = 0** | **ToS = 1** | **ToS = 2** |
|--|--|--|--|
| 1 | 90% | 5% | 5% |
| 2 | 33,3% | 33,3% | 33,3% |
| 3 | 60% | 30% | 10% |
| 4 | 50% | 40% | 10% |
| 5 | 75% | 25% | 5% |

ToS are assigned to flows with the following probability: 10% for ToS=0, 30% for ToS=1 and 60% for ToS=2.

  
**Scenario 3:** In this scenario each node is configured randomly with one of the queue scheduling policies (SP, WFQ or DRR). For WFQ and DRR, we also define two sets with 5 queue weights profiles respectively. A total of 100 scheduling configurations (i.e., nodes + policies + weight profiles) are generated with this criterion for each topology, and each simulation selects randomly one of these configurations.

  
**WFQ Profiles**

|  **Weight profile**| **ToS = 0** | **ToS = 1** | **ToS = 2** |
|--|--|--|--|
| 1 | 90% | 5% | 5% |
| 2 | 33,3% | 33,3% | 33,3% |
| 3 | 60% | 30% | 10% |
| 4 | 50% | 40% | 10% |
| 5 | 75% | 25% | 5% |

**DRR Profiles**
|  **Weight profile**| **ToS = 0** | **ToS = 1** | **ToS = 2** |
|--|--|--|--|
| 1 | 80% | 10% | 10% |
| 2 | 33,3% | 33,3% | 33,3% |
| 3 | 60% | 30% | 10% |
| 4 | 70% | 20% | 10% |
| 5 | 65% | 25% | 10% |

ToS are assigned to flows with the following probability: 10% for ToS=0, 30% for ToS=1 and 60% for ToS=2

**Scenario 4:** This scenario is like scenario 3, but ToS are assigned to flows equiprobably (i.e., 33,3% each one).  
  
### Routing configuration

For each topology, we define 100 routing configurations which are variations of shortest path. For each simulation, one routing is selected randomly between the 100 candidates.

> It is highly recommended to use the API provided [[here](https://github.com/knowledgedefinednetworking/datanetAPI/tree/challenge2020)] to easily read and process samples from the dataset. However, if you prefer to process directly the raw data, you can find the description of the dataset format in the section below.

## Processing the dataset from raw data

The root directory of the compressed files contains the ‘routings’ directory where routing configuration files are stored. These files include a matrix describing the destination-based Routing Information Base (RIB) at each node:  
Routing_matrix(src node , dst node) = output port to reach the dst node from the src node.

  
In the ‘graphs’ directory we locate the topologies and their features associated. Each topology file describes the nodes and links of the topology in Graph Modeling Language (GML) including the queue scheduling policy used for each node. This file can be processed with the networkx library using the read_gml method:

  
`G= networkx.read_gml(topology_file, destringizer=int)`  
  
Finally, we have a set of compressed files with 100 simulation samples each one. Each of these files contain the following data:

- input_files.txt: Each line of this file contains the simulation number, the topology file, and the routing file used for that simulation.

- traffic.txt: Contains the traffic parameters used by the simulator to generate the traffic for each iteration. At the beginning of each line we have the maxAvgLambda selected for this iteration. This parameter is separated from the rest of the information with the ‘|’ character. The rest of the line corresponds to the parameters used to generate the traffic for each path. Paths information are separated with semicolon (;) and the parameters used for those paths are separated with commas (,). The parameters of each path depend on the time an packet size distribution used and is structured as follows:

      <time_distibution>,<equivalent_lambda>, <time_dist_param_1>,..,<time_dist_param_n>,<size_distibution>, <avg_pkt_size>, <size_dist_param_1>,..,<size_dist_param_n><ToS>.

  
  For this dataset the time_distribution is always Poisson (i.e., <time_distibution>=0) and the size distribution is binomial (i.e., <size_distibution>=2).

  The rest of parameters for the Poisson/Exponential distribution are:  
    - Avg number of packets per time unit considering packets of avg_pkt_size.  
    - Exponential max factor: Factor used to define an upper bound for exponential distributions. The upper bound is defined as: ‘ExpMaxFactor’* equivalent_lambda.

  The rest of parameters for the binomial distribution are:  
    - Packet size 1: First packet size option (bits).  
    - Packet size 2: Second packet size option (bits).
* simulationResults.txt: Contains the measurements obtained by our network simulator for every sample. Each line in ‘simulationResults.txt’ corresponds to a simulation using the topology, routing and queue scheduling configuration specified in the ‘input_files.txt’, and the input traffic matrices specified in the ‘traffic.txt’ file.
At the beginning of the line, and separated by “|”, there are global network statistics separated by commas (,). These global parameters are:

      1.- global_packets: Number of packets transmitted in the network per time unit (packets/time unit).
      2.- global_losses: Packets lost in the network per time unit (packets/time unit). 
      3.- global_delay: Average per-packet delay over all the packets transmitted in the network (time units)
   After the “|” and separated by semicolon (;) we have the list of all path. Finally the metrics of the related to a path are separated by semicolon (;). Likewise, the different measurements (e.g., delay, jitter) for each path are separated by commas (,). So, to obtain a pointer to the metrics of a specific path from ‘node_src’ to ‘node_dst’, one can split the CSV format considering the semicolon (;) as separator:

      1.- Bandwidth (in kbits/time unit) trasmitted in each source-destination pair in the network (in both directions). 
      2.- Absolute number of packets transmitted in each src-dst pair (in both directions).  
      3.- Absolute number of packets dropped in each src-dst pair.  
      4.- Average per-packet delay over the packets transmitted in each src-dst pair (in time units).  
      5.- Average neperian logarithm of the per-packet delay over the packets transmitted src-dst pair (in time units). 
      6.- Percentile 10 of the per-packet delay over the packets transmitted in each src-dst pair (in time units).  
      7.- Percentile 20 of the per-packet delay over the packets transmitted in each src-dst pair (in time units).  
      8.- Percentile 50 (median) of the per-packet delay over the packets transmitted in each src-dst pair (in time units).   
      9.- Percentile 80 of the per-packet delay over the packets transmitted in each src-dst pair (in time units).  
      10.- Percentile 90 of the per-packet delay over the packets transmitted in each src-dst pair (in time units).  
      11.- Variance of the per-packet delay (jitter) over the packets transmitted in each source-destination pair.  

*   stability.txt: Contains some extra information used to evaluate the status of the dataset. The more relevant parameter from this file is the simulation time required to reach the stability condition which, for each simulation, is the first element of the line.  
      
The datasets can be downloaded from these links:
    
|**File**  | **Size** | **MD5SUM** |
|--|--|--|
|[Training Dataset](https://bnn.upc.edu/download/ch20-training-dataset/) | 5,97GB | f0060fba6b4ac9f761d58799d5b555d0 |
|[Validation dataset](https://bnn.upc.edu/download/ch20-validation-dataset/) | 1,12GB | ab0a215e18577f7964cd1431387a3f68 |
|[Test dataset](https://bnn.upc.edu/download/ch20-evaluation-dataset/) | 289MB | 13f2224276d6a05c3bd7e499e8e2dac1 |
|[Test dataset (with labels)](https://bnn.upc.edu/download/ch20-evaluation-complete-dataset/) | 689MB | da183826f9274c310564b9e1616e6f43 |


Please, if you would like to be notified with new dataset releases or discuss anything related to the datasets, you can also subscribe to the mailing list kdn-users@knowledgedefinednetworking.org (Link: https://mail.knowledgedefinednetworking.org/cgi-bin/mailman/listinfo/kdn-users).

<br>
This dataset is part of the Spanish I+D+i project TRAINER-A (ref. PID2020-118011GB-C21), funded by MCIN/AEI/10.13039/501100011033.
<p align="left"> 
  <img src="/assets/logo_ministerio.png" width="500" alt>
</p> 
