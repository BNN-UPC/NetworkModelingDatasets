## Dataset description

This repository contains datasets with simulation results of delay, jitter and packet loss for three different network topologies: NSFNET (14 nodes), GBN (17 nodes) and GEANT2 (24 nodes) plus a few samples for validation purpose using topologies of Topology ZOO. 

The training dataset contains samples simulated in the NSFNET and GEANT2 network topologies, while the validation dataset contains samples simulated in GBN.

These datasets have their focus on adding queue scheduling policies that may be complex to model. To this end, we simulate scenarios where devices are configured with different scheduling policies. The following policies policies are used: First In First Out (FIFO), Strict Priority (SP), where packets in queues with more priority are transmitted first, Weighted Fair Queueing (WFQ), and Deficit Round Robin (DRR).

For all the queue scheduling policies implemented (except FIFO), we consider between two and five queues with different priorities on nodes. For each queue convination, we define 5 different weight policies. We have 10 different Type of Services that are mapped to the priority queues of the node using five different mappings per number of queues of the node.  At the end we define 2000 configurations per topology where on each of these configurations we select randomly for each of the nodes the scheduling policy, number of queues, the weights of the queue and the mapping of the ToS to the priority queue.

At the beginning of each simulation, a ToS is associated to the flows generated in a source-destination path. All the packets of the path will have the same ToS. Packets are generated on each flow following a Poisson distribution. To this end, we use an exponential time distribution (TimeDist.EXPONENTIAL_T) to model inter-packet arrival times. Likewise, we use a binomial distribution (SizeDist.BINOMIAL_S) to model packet size. The maximum bitrate that paths may have (maxAvgLambda) is selected randomly for each simulation, between 400 and 2000 bits per time unit. Note that packet delay is limited to 20 time units for all the simulation scenarios.

For each topology, we define 100 routing configurations which are variations of shortest path. For each simulation, one routing is selected randomly between the 100 candidates.

### Topology ZOO dataset

To experiment with the generalization capabilities of the GNN, we also provide a few samples simulated in 106 real-world network topologies from the Internet Topology Zoo project [1]. The scheduling policy configuration has been generated following the same procedure described in the previous section. 10 scheduling policy configurations and 10 variations of SP routing have been generated for each topology. The final dataset is made up of 12750 samples generated randomly from these 106 topologies.

### Real traces dataset

The last dataset pretends to use more realistic traffic matrices. We use real-world traffic matrices from SNDlib library [2] scaled to the maximum intensity demand generated in the training dataset . Since the traffic matrices only contain traffic aggregation of each source-destination pair, we use a recent snapshot from the MAWI repository (SamplePoint-F,Oct. 2020) [3] to extract realistic packet inter-arrival times. Then, we scale these inter-arrivals according to the values in the traffic matrices. Regarding the mapping of source-destination flows to ToS classes, we follow the same distribu-tion from a real ISP [4]. Finally, we simulate this scenario in three different topologies: GBN (Nobel), GEANT, and ABILENE, extracting 1000 samples for each one. The remaining simulation parameters are the same as in the previous sections.

## Processing the dataset

It is highly recommended to use the API provided [[here](https://github.com/BNN-UPC/datanetAPI/tree/dataset_v4)] to easily read and process samples from the dataset. However, if you prefer to use directly the raw data, you can find the description of the dataset format below.

  
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

## Downloads
The datasets can be downloaded from these links:
    
|**File**  | **Size** | **MD5SUM** |
|--|--|--|
|[NSFnet Dataset](https://bnn.upc.edu/download/dataset-v4-nsfnet/) | 774 MB | 1c5028266f9ffa6e74cd42a27ee3fb5c |
|[GEANT2 Dataset](https://bnn.upc.edu/download/dataset-v4-geant2/) | 2.3 GB | 57d7b8d5884876602a5884836db59360 |
|[GBN Dataset](https://bnn.upc.edu/download/dataset-v4-gbn/) | 1.2 GB | 265968bc7343ad8d12fcf205956fa4c2 |
|[Topology ZOO](https://bnn.upc.edu/download/dataset-v4-topology-zoo/) | 774 MB | fbc629c91de74dc87b41099a5701a091 |

#### Real traces
|**File**  | **Size** | **MD5SUM** |
|--|--|--|
|[Abilene Dataset](https://bnn.upc.edu/download/dataset-v4-abilene-sndlib/) | 20 MB | 6d67068fc498470e57ddc5b530bec363 |
|[NOBEL-Germany Dataset](https://bnn.upc.edu/download/dataset-v4-nobel-sndlib/) | 24 MB | 0b618ff1d39d3d5b96c29327583e8e51 |
|[GEANT Dataset](https://bnn.upc.edu/download/dataset-v4-geant-sndlib/) | 27 MB | 7c420c395b245b8a1fbfe7474a217e6d |


### References
[1] The Internet Topology Zoo: [http://www.topology-zoo.org](http://www.topology-zoo.org)
[2] SNDlib: [http://sndlib.zib.de/home.action](http://sndlib.zib.de/home.action)
[3] MAWI repository: [https://mawi.wide.ad.jp/mawi/](https://mawi.wide.ad.jp/mawi/)
[4] S. Schnitteret al., “Quality-of-service class specific traffic matrices inip/mpls networks,” inACM Internet Measurement Conference, 2007, pp.253–258.


Please, if you would like to be notified with new dataset releases or discuss anything related to the datasets, you can also subscribe to the mailing list kdn-users@knowledgedefinednetworking.org (Link: https://mail.knowledgedefinednetworking.org/cgi-bin/mailman/listinfo/kdn-users).


