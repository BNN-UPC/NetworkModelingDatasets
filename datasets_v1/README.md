# Datasets v1

> Note: It is highly recommended to use the API provided [here](https://github.com/knowledgedefinednetworking/datanetAPI/tree/dataset_v1) to easily read and process samples from the dataset. However, if you prefer to use directly the raw data, you can find the description of the dataset format below.

> These datasets were used in the following paper:<br>
> Rusek, Krzysztof, _et al_. "RouteNet: Leveraging Graph Neural Networks for network modeling and optimization in SDN." _IEEE Journal on Selected Areas in Communications_ 38.10 (2020): 2260-2270.<br>
> Link to paper \[ArXiv\]: https://arxiv.org/abs/1910.01508<br>
> Link to code repository: https://github.com/knowledgedefinednetworking/net2vec/tree/RouteNet-JSAC19/routenet

### Dataset description

This repository contains datasets with simulation results of delay, jitter and packet loss in four different network topologies. It is divided in four compressed files (.tar.gz) corresponding to different topologies. Each file follows the structure below:

* *.ned file: Describes the network topology including the link capacities.

* Compressed files (.tar.gz) with the simulation results. Each file contains samples simulated in network scenarios with a particular routing scheme and a given traffic intensity. Particularly, the filenames use the following structure:             
      
      results_<topology_name>_<lower lambda max>-<upper lambda max>_<routing_scheme>_0_124.tar.gz

    Where \<lower lambda max>-\<upper lambda max> represents the range of traffic intensity and <routing_scheme> is an identifier of the routing configuration used in the simulations.

    Each of these files contain the following data:

    - Routing.txt: Contains the routing scheme used in the simulations. This file includes a matrix describing the destination-based Routing Information Base (RIB) at each node:

          Routing_matrix(src node , dst node) = output port to reach the dst node from the src node

    - simulationResults.txt: Contains the samples generated by our network simulator (125 samples in each file). Each line in 'simulationResults.txt' corresponds to a simulation with different input traffic matrices probabilistically generated in the range of traffic intensities [\<lower boundary lambda max\>, \<upper_boundary lambda max\>]. All the samples are simulated considering the routing scheme defined in 'Routing.txt'. We describe below the data structure of each line (i.e., each simulation). Note that in a topology with ‘n’ nodes, nodes are enumerated in the range [0,n-1]. Then, the indices 'src_node' and 'dst_node' used below are in the range [0,n-1].
  
      1.- Average traffic rate (in kbps) transmitted in each source-destination pair.
  
             traffic[src_node][dst_node] = line[(src_node∗n+dst_node)*3]
  
      2.- Total number of packets transmitted in each source-destination pair during the simulation.
  
             pkts_transmitted[src_node][dst_node] = line[(src_node∗n+dst_node)*3 + 1]
  
      3.- Total number of packets dropped in each source-destination pair during the simulation.
  
             pkts_dropped[src_node][dst_node] = line[(src_node∗n+dst_node)*3 + 2]
  
      4.- Average packet delay for the packets transmitted in each source-destination pair. 
  
             avg_delay[src_node][dst_node] = line[n*n*3 + (src_node∗n+dst_node)*8]
  
      5.- Average per-packet neperian logarithm of the delay over the packets transmitted in each source-destination pair.
  
             avg_ln_delay[src_node][dst_node] = line[n*n*3 + (src_node∗n+dst_node)*8 + 1]
  
      6.- Percentile 10 of the per-packet delay over the packets transmitted in each source-destination pair.
  
             delay_p_10[src_node][dst_node] = line[n*n*3 + (src_node∗n+dst_node)*8 + 2]
  
      7.- Percentile 20 of the per-packet delay over the packets transmitted in each source-destination pair.
  
             delay_p_20[src_node][dst_node] = line[n*n*3 + (src_node∗n+dst_node)*8 + 3]
  
      8.- Percentile 50 (median) of the per-packet delay over the packets transmitted in each source-destination pair.
  
             delay_p_50[src_node][dst_node] = line[n*n*3 + (src_node∗n+dst_node)*8 + 4]
  
      9.- Percentile 80 of the per-packet delay over the packets transmitted in each source-destination pair.
  
             delay_p_80[src_node][dst_node] = line[n*n*3 + (src_node∗n+dst_node)*8 + 5]
  
      10.- Percentile 90 of the per-packet delay over the packets transmitted in each source-destination pair.
  
             delay_p_90[src_node][dst_node] = line[n*n*3 + (src_node∗n+dst_node)*8 + 6]
  
      11.- Variance of the per-packet delay (jitter) over the packets transmitted in each source-destination pair.
  
             jitter[src_node][dst_node] = line[n*n*3 + (src_node∗n+dst_node)*8 + 7]
    
    - params.ini: Contains some input parameters used in our simulator. The most relevant ones at the user-level are 'simulationDuration' and 'repeat'. 'simulationDuration' is the total duration of the simulation (in relative time units). For instance, this value is used to tune the simulation time in order to ensure that the behavior of performance metrics such as delay reachs a stationary state. Note that some simulation results like the total number of packets transmitted or dropped depend also on this parameter. Likewise, the 'repeat' parameter defines the number of simulation samples included in the current 'tar.gz' file.
    
* tfrecords/train: This directory contains the TFRecords files with the samples we used to train RouteNet in our [Demo paper](https://github.com/knowledgedefinednetworking/demo-routenet). To generate this training set, we randomly selected 80% of the samples in the original dataset.

* tfrecords/evaluate: This directory contains the TFRecords files used to evaluate RouteNet in our [Demo paper](https://github.com/knowledgedefinednetworking/demo-routenet). It contains a collection with the remaining 20% of the samples that were not present in the training set.
    
All the datasets can be downloaded from these links:

* NSFNet topology: https://knowledgedefinednetworking.org/data/datasets_v1/nsfnetbw.tar.gz
<p align="center"> 
  <img src="/assets/nsfnet_topology.png" width="400" alt>
</p>

* GBN topology: https://knowledgedefinednetworking.org/data/datasets_v1/gbnbw.tar.gz
<p align="center"> 
  <img src="/assets/gbn_topology.png" width="400" alt>
</p>

* GEANT2 topology: https://knowledgedefinednetworking.org/data/datasets_v1/geant2bw.tar.gz
<p align="center"> 
  <img src="/assets/geant2_topology.png" width="400" alt>
</p>

* germany50 topology: https://knowledgedefinednetworking.org/data/datasets_v1/germany50bw.tar.gz
<p align="center"> 
  <img src="/assets/germany50_topology.png" width="400" alt>
</p>

Please, if you would like to be notified with new dataset releases or discuss anything related to the datasets, you can also subscribe to the mailing list kdn-users@knowledgedefinednetworking.org (Link: https://mail.knowledgedefinednetworking.org/cgi-bin/mailman/listinfo/kdn-users).
