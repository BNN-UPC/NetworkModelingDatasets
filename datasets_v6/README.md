# Datasets v6

> Note 1: This dataset has been generated with [BNNetSimulator](https://github.com/BNN-UPC/BNNetSimulator) 

> Note 2: To facilitate reading and processing of the dataset, we recommend using the API provided [[here](https://github.com/BNN-UPC/datanetAPI/tree/BNNetSimulator)]. The API is included within all the datasets for ease of use. However, if you prefer to process the raw data directly, you can find the description of the dataset format below.  

## Dataset description
This repository contains a collection of datasets that contain simulation results for delay, jitter, and packet drops. These results were obtained in the context of the publication "RouteNet-Fermi: Network Modeling with Graph Neural Networks" [1], and are intended to be used as a benchmark for network modeling and optimization. The datasets provided can be splitted into three categories:  

 - Scheduling policies
 - Scalability
 - Traffic profiles

### Scheduling policies:
The datasets in this repository cover different scheduling policies used in computer networking. These policies include First In First Out (FIFO), Strict Priority (SP), Weighted Fair Queueing (WFQ), and Deficit Round Robin (DRR).

For all queue scheduling policies (except FIFO), we use three queues with different priorities on nodes. Additionally, WFQ and DRR datasets define five different weight policies. To configure each topology, we select a scheduling policy for each node. If WFQ or DRR is selected, we also choose a weight policy.

At the beginning of each simulation, we select for each source-destination path a Type of Service (ToS) between 0 to 2. All packets within the same path have the same ToS.

Some datasets also have different buffer sizes per node. The scheduling policies and buffer sizes are randomly selected but remain the same for all the ports of a node.

The provided datasets can be used to evaluate the performance of different scheduling policies and buffer sizes on network performance.
### Scalability:
The datasets used in our study of this problem are divided into two categories: training datasets and validation datasets. The training datasets contain topologies with less than 50 nodes, while the validation datasets include topologies with up to 300 nodes.
### Traffic profiles:
The datasets used in our study also include a set of traffic profiles, which are designed to help researchers understand how the network behaves under different types of traffic. The evaluated traffic profiles fall into two main categories: time distribution profiles and size distribution profiles.

The time distribution profiles are designed to simulate different patterns of traffic over time. These profiles include:

 - **Poisson**
 - **Constant Bit Rate (CBR)**
 - **Autocorrelated exponentials:** generates autocorrelated exponentially distributed traffic starting from the following auto-regressive (AR) process: zt+1=azt+ε, ε∼N(0, σ2) where a∈(−1, 1) controls the level of autocorrelation [2].
 - **Modulated exponentials:** is an alternative autocorrelated model with higher complexity for QT than the Autocorrelated exponentials [2].
 - **Real traces:** uses time between packets extracted from actual network traffic.

The size distribution profiles describe the distribution of packet sizes in the traffic. These profiles include:

 - **Deterministic:** all packets have the same size (1000 bits).
 - **Binomial:** packet size can be either 300 or 1700 bits with equal probability
 - **Generic:** allows researchers to define a set of packet sizes and their probabilities of being  
selected
 - **Real traces:** uses packet sizes obtained from actual network traffic.
## Datasets

### Fat tree datasets

 - **Topologies:** fat-tree-16, fat-tree-64 and fat-tree-128
 - **Routings:** 1 candidate per topology
 - **Scheduling policy:** FIFO
 - **Queue size:** 32000 bits
 - **Traffic profile:**
	 - *Time distribution:* Poisson
	 - *Size distribution:* Binomial between 300 and 1700 bits
 - **Traffic matrix:** To generate traffic for each sample, a load is selected. The traffic is then randomly distributed among all paths, with the load calculated as the product of the link speed, number of cores, number of clusters, and load factor.

#### Downloads:
|**File**  | **Size** | **MD5SUM** |
|--|--|--|
|[fat-tree-16 dataset](https://bnn.upc.edu/download/dataset-v6-fat-tree-16/) | 11 MB | a1a14d78e1a0a887a6e0420e26dffc96 |
|[fat-tree-64 dataset](https://bnn.upc.edu/download/dataset-v6-fat-tree-64/) | 165 MB | 14b732ed70b33f7adb87b2da47bffa7a |
|[fat-tree-128 dataset](https://bnn.upc.edu/download/dataset-v6-fat-tree-128/) | 620 MB | 6116bdb8a6f54b9b9bb92de1b429405d |

### Scalability dataset:

 - **Topologies:**
	 - Training: Two random topologies for each topology size: 25, 30, 35, 40, 45 & 50
	 - Test: Two random topologies for each topology size: 50, 55, 60, 65, 70, 75, 80, 85, 90, 95, 100, 110, 120, 130, 140, 150, 160, 170, 180, 190, 200, 220, 240, 260 & 300
 - **Routings:** 50 variations of shortest path for training topologies and 20 for test topologies
 - **Scheduling policy:** FIFO
 - **Queue size:** 32 packets
 - **Traffic profile:**
	 - *Time distribution:*Poisson
	 - *Size distribution:* Binomial between 300 and 1700 bits
 - **Traffic matrix:** For each sample, the maximum average lambda (maxAvgLbda) is selected between 400 and 2000 bps. The bandwidth per path is then determined by multiplying maxAvgLbda by a randomly generated uniform value between 0.1 and 1.

#### Download
|**File**  | **Size** | **MD5SUM** |
|--|--|--|
|[Scalability dataset](https://bnn.upc.edu/download/dataset-v6-scalability/) | 7.09 GB | 4c60b67c0a91bee0b3160e182ad313b0 |

### Traffic Model dataset:

 - **Topologies:**
	 - Training: NSFNET (14 nodes) and GEANT2 (24 nodes)
	 - Test: GBN (17 nodes)
 - **Routings:** 100 variations of shortest path per topology
 - **Scheduling policy:** FIFO
 - **Queue size:** 32 packets
 - **Traffic profile:**
	 - *Time distribution:*
		 - Subdataset 1 : Constant Bit Rate
		 - Subdataset 2 : ON-OFF
		 - Subdataset 3 : Autocorrelated exponentials
		 - Subdataset 4 : Modulated exponentials
		 - Subdataset 5: Mix of CBR, ON-OFF, Autocorrelated and Modulated
	 - *Size distribution:* All subdatasets use binomial between 300 and 1700 bits
 - **Traffic matrix:** For each sample in the dataset, the maximum average lambda (maxAvgLbda) is selected randomly between 400 and 2000 bps. Then, the bandwidth per path is determined by multiplying maxAvgLbda by a uniform random variable between 0.1 and 1.

#### Download
|**File**  | **Size** | **MD5SUM** |
|--|--|--|
|[Traffic models dataset](https://bnn.upc.edu/download/dataset-v6-traffic-models/) | 12.13 GB | 3817fc4ba95c41375de073802b4c84f8 |

### Scheduling dataset:

 - **Topologies:**
	 - Training: NSFNET (14 nodes) and GEANT2 (24 nodes)
	 - Test: GBN (17 nodes)
 - **Routings:** 100 variations of shortest path per topology
 - **Scheduling policy:** 100 configurations with WFQ and 100 configurations with SP, WFQ and DRR
 - **Queue size:** 32 packets
 - **Traffic profile:**
	 - *Time distribution:* Poisson
	 - *Size distribution:* Binomial between 300 and 1700 bits
 - **Traffic matrix:** For each sample, the maximum average lambda (maxAvgLbda) is selected randomly between 400 and 2000 bps. The bandwidth per path is then determined by multiplying maxAvgLbda with a random value generated from a uniform distribution between 0.1 and 1. Additionally, the Type of Service (ToS) is selected randomly for each path from the values 0, 1, and 2.

#### Download
|**File**  | **Size** | **MD5SUM** |
|--|--|--|
|[Scheduling dataset](https://bnn.upc.edu/download/dataset-v6-scheduling/) | 6.64 GB | 29d81c60d25a7495b365c9bf6939333b |

### All mixed dataset:

 - **Topologies:**
	 - Train: 7 random topologies of 10 nodes
	 - Test: 2 random topologies per each of the following network sizes: 50, 75, 100, 130, 170, 200, 240, 260, 280 & 300
	 - Validation: 1 random topology for each of the following network size: 100, 150, 200, 260 & 300
 - **Routings:** 10 variations of shortest path per topology
 - **Scheduling policy:** 
	 - Training: 50 configurations with FIFO, SP, WFQ and DRR
	 - Test: 5 configurations with FIFO, SP, WFQ and DRR
	 - Validation: 5 configurations with FIFO, SP, WFQ and DRR
 - **Queue size:** Selected randomly for each node between, 8000, 16000, 32000 and 64000 bits. 
 - **Traffic profile:**
	 - *Time distribution:* Selected randomly per path between Poisson, CBR and ON-OFF
	 - *Size distribution:* Generic distribution with 5 packet size candidates and its probabilities. We have 5 different packet size distributions and we select one for each sample.
 - **Traffic matrix:** For each sample, select maximum average lambda (maxAvgLbda) between 400 and 2000 bps. The bandwidth per path is selected by multiplying maxAvgLbda per uniform between 0.1 and 1. ToS is selected randomly per path between 0,1 and 2.

#### Download
|**File**  | **Size** | **MD5SUM** |
|--|--|--|
|[All mixed dataset](https://bnn.upc.edu/download/dataset-v6-all-mixed/) | 1.6 GB | 00c766ff01aeca21e2e1b2b9bcf18868 |

### Real traces dataset:

 - **Topologies:**
	 - Train: GEANT (24 nodes)
	 - Test: ABILENE (12 nodes), GEANT (22 nodes), GERMANY50 (50 nodes) and NOBEL (17 nodes)
 - **Routings:**26 variations of shortest path per topology
 - **Scheduling policy:** 100 configurations with FIFO, SP, WFQ and DRR
 - **Queue size:** Selected randomly for each node between, 8000, 16000, 32000 and 64000 bits.
 - **Traffic profile:** Inter packet gap and packet size is extracted from real trace from MAWI repository (Sample point 2022/09) [3]. We then scale the inter-arrival times to match the values in the traffic matrix 
	 - *Time distribution:* Real dataset
	 - *Size distribution:* Real dataset
 - **Traffic matrix:** The used traffic matrices are extracted from sndlib library [4]. To make it computationally feasible, the matrices are scaled in a factor of 1000 (Mbits values are considered to be Kbits values)

#### Download
|**File**  | **Size** | **MD5SUM** |
|--|--|--|
|[Real traces dataset](https://bnn.upc.edu/download/dataset-v6-real-traces/) | 50 MB | 6e7c4518e5e9ec74f664154cc5934ac9 |

## Processing the dataset from raw data

We highly recommend using the provided API to read and process samples from the dataset easily. However, if you prefer to work with the raw data directly, you can find the dataset format description below. Some datasets in this repository consist of multiple sub-datasets, such as training and validation datasets. Each sub-dataset contains a 'routings' directory and a 'graphs' directory.

The 'routing' folder stores routing configuration files, which contain a matrix describing the destination-based Routing Information Base (RIB) at each node. This matrix specifies the output port required to reach the destination node from the source node.

Routing_matrix(src node , dst node) = output port to reach the dst node from the src node.

In the 'graphs' directory, you can find the topologies and their associated features. Each topology file is written in Graph Modeling Language (GML) and describes the nodes, links, and queue scheduling policy used for each node. You can process this file with the networkx library using the read_gml method:

`G= networkx.read_gml(topology_file, destringizer=int)`

The compressed files in our dataset contain multiple simulation samples, and the number of samples in each file depends on the dataset. Each compressed file includes the following data:
### input_files.txt:
This file contains a list of simulation numbers, topology files, and routing files used for each simulation. Each line corresponds to one simulation.

### traffic.txt:
This file contains the traffic parameters used by the simulator to generate traffic for each iteration (one iteration per line). The file starts with the maxAvgLambda selected for the iteration, separated from the rest of the information by the '|' character. The parameters used to generate traffic for each path are separated by semicolons, and the parameters for each path include time and packet size distributions. The structure of these parameters is as follows:

`<time_distribution>,<equivalent_lambda>,<time_dist_param_1>,..,<time_dist_param_n>,<size_distribution>,<avg_pkt_size>,<size_dist_param_1>,..,<size_dist_param_n><ToS>`

Note that the specific parameters for each distribution depend on the type of distribution used.

####  Time distribution parameters:

 - Poisson distribution:
	 - time_distribution = 0
	 - equivalent_lambda: Avg umber of bits per time unit
	 - Avg number of packets per time unit considering packets of avg_pkt_size
	 - Exponential max factor: Factor used to define an upper bound for exponential distributions. The upper bound is defined as: ‘ExpMaxFactor’* equivalent_lambda
 - Constant Bit Rate (CBR):
	- time_distribution = 1
	- equivalent_lambda: Avg number of bits per time unit
	- Avg number of packets per time unit considering packets of avg_pkt_size
 - On-Off distribution:
	- time_distribution = 4
	- equivalent_lambda: Avg number of bits per time unit
	- Avg number of packets per time unit considering packets of avg_pkt_size
	- Avg time off: Average duration of the OFF period. The duration of every OFF period is modeled with an exponential distribution of average = ‘AvgTOff’. (time units)
	- Avg time on: Average duration of the ON period. The duration of every ON period is modeled with an exponential distribution of average = ‘AvgTOn’. (time units)
	- Exp Max Factor: Factor used to define an upper bound for exponential distributions. The upper bound is defined as: ‘ExpMaxFactor’* Avg time on/off
 - Trace distribution:
	- time_distribution = 6
	- equivalent_lambda: Avg number of bits per time unit
 - Autocorrelated exponentials:
	- time_distribution = 7
	- equivalent_lambda: Avg umber of bits per time unit
	- Name of the distribution: “generate_autosimilar”
	- AR1-0 parameter
	- AR-a parameter
	- Samples
 - Modulated exponentials:
	- time_distribution = 7
	- equivalent_lambda: Avg umber of bits per time unit
	- Name of the distribution: “autosimilar_k2”
	- AR1-1 parameter
	- Sigma
	- Samples
 
####  Size distribution parameters:
 - Deterministic:
	 - size_distibution = 0
	 - Packet size
 - Binomial:
	 - size_distribution = 2
	 - avg_pkt_size
	 - First packet size option
	 - Second packet size option 
 - Generic:
	 - size_distribution = 3
	 - avg_pkt_size
	 - Number of candidates: : Number of different packets size considered.
	 - Size of the candidate packet 1
	 - Probability to select candidate packet 1
	 - ...
	 - Size of the candidate packet n
	 - Probability to select candidate packet n
 - Trace:
	 - size_distribution = 4
	 - avg_pkt_size

### simulationResults.txt:
Contains the measurements obtained by our network simulator for every sample. Each line in ‘simulationResults.txt’ corresponds to a simulation using the topology, routing and queue scheduling configuration specified in the ‘input_files.txt’, and the input traffic matrices specified in the ‘traffic.txt’ file. At the beginning of the line, and separated by “|”, there are global network statistics separated by commas (,). These global parameters are:

 1. global_packets: Number of packets transmitted in the network per time unit (packets/time unit).
 2. global_losses: Packets lost in the network per time unit (packets/time unit). 
 3. global_delay: Average per-packet delay over all the packets transmitted in the network (time units)

After the “|” and separated by semicolon (;) we have the list of all path. Finally the metrics of the related to a path are separated by commas (,) (e.g., delay, jitter). So, to obtain a pointer to the metrics of a specific path from ‘node_src’ to ‘node_dst’, one can split the CSV format considering the semicolon (;) as separator:

 1. Average trafic rate (in kbits/time unit) trasmitted in each source-destination pair in the network (in both directions)
 2. Absolute number of packets transmitted in each src-dst pair (in both directions)
 3. Absolute number of packets dropped in each src-dst pair
 4. Average per-packet delay over the packets transmitted in each src-dst pair (in time units)
 5. Average neperian logarithm of the per-packet delay over the packets transmitted src-dst pair (in time units)
 6. Percentile 10 of the per-packet delay over the packets transmitted in each src-dst pair (in time units)
 7. Percentile 20 of the per-packet delay over the packets transmitted in each src-dst pair (in time units)
 8. Percentile 50 (median) of the per-packet delay over the packets transmitted in each src-dst pair (in time units)
 9. Percentile 80 of the per-packet delay over the packets transmitted in each src-dst pair (in time units)
 10. Percentile 90 of the per-packet delay over the packets transmitted in each src-dst pair (in time units)
 11. Variance of the per-packet delay (jitter) over the packets transmitted in each source-destination pair

### stability.txt:

Contains some extra information used to evaluate the status of the dataset. The more relevant parameter from this file is the simulation time required to reach the stability condition which, for each simulation, is the first element of the line.

### linkUsage.txt:

Contain the performance metrics of the output ports of the node. Separated by semicolon (;) we have the list of all possible links / ports , src node, dst node. Likewise, the different statistics of the port are separated by commas (,). If no link exists between src and dst node, instead a list of parameters, a value -1 is set.
The parameters separeted by comma are:

 1. The average utilization of the outgoing port (in the range [0, 1]).
 2. Average packets lost in the outgoing port (in the range [0, 1]).
 3. Average packet size from all outgoing packets going through the port (bits).

Then we have a colon (:) followed by a list of parameters related to each of the QoS queues. Queues are separated with colons (:) and its parameters are separated by comma (,). The list of a parameters of the QoS queues are the following ones:

 1. The average utilization of the outgoing port (in the range [0, 1]).
 2. Average packets lost in the outgoing QoS queue (in the range [0, 1]).
 3. Average port occupancy in packets or bits depending if the buffer size is specified in packets or bits
 4. Maximum occupancy seen on the QoS queue in packets or bits depending if the buffer size is specified in packets or bits
 5. Average packet size from all outgoing packets going through the queue (bits).

## References
[1] Miquel Ferriol-Galmés, Jordi Paillisse, José Suárez-Varela, Krzysztof Rusek, Shihan Xiao, Xiang Shi, Xiangle Cheng, Pere Barlet-Ros, Albert Cabellos-Aparicio, “**RouteNet-Fermi: Network Modeling with Graph Neural Networks**”

[2] Miquel Ferriol-Galmés, Krzysztof Rusek, José Suárez-Varela, Shihan Xiao, Xiangle Cheng, Pere Barlet-Ros, Albert Cabellos-Aparicio, “**RouteNet-Erlang: A Graph Neural Network for Network Performance Evaluation**” in IEEE INFOCOM 2022

[3] SNDLIB: http://sndlib.zib.de/home.action

[4] MAWI REPOSITORY:  https://mawi.wide.ad.jp/mawi/
