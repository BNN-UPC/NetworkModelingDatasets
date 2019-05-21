# Datasets

In this repository you can find the links to download datasets for different network topologies. Each topology corresponds to a compressed file with the following structure:

* *.ned file that describes the network topology. We consider that all the links in the topology have the same capacity.

* Routing: The /routing* directory contains the different routing schemes simulated. Each routing file includes a matrix that describes the destination-based Routing Information Base (RIB) at each node:

      Routing_matrix(src node , dst node) = output port to reach the dst node from the src node.

* Output performance metrics: The /delays* directory contains the simulation results. This includes per source-destination delays, jitter and packet drops. The filenames use the following structure:             
      
      dGlobal_0_<lambda>_<routing_file>.txt

Where <lambda> is the traffic intensity and <routing_file> indicates the routing scheme used in the simulation. Each line in the dGlobal_0_*.txt files corresponds to a simulation with different traffic matrices probabilistically generated from a given traffic intensity (lambda). We describe below the data structure of each line (i.e., each simulation). Note that in a topology with ‘n’ nodes, nodes are enumerated in the range [0, n-1].
  
  1. Bandwidth generated for each source-destination pair in the network. This is a flattened vector with nxn fields, where the index for a particular src-dst pair is:
  
         index=srcnode∗n+dstnode

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;E.g.: The bw generated for the pair 0-7 in a network with 10 nodes would be in the position (index = 0*10+7 = 7)
  
  2. Average delay over the packets transmitted for each source-destination pair in the network. This contains nxn values. This vector and the subsequent have the same indexes for nodes than the previous vector.
  
  3. Percentile 10 over the packets transmitted for each source-destination pair.  This contains nxn values.
  
  4. Percentile 20 over the packets transmitted for each source-destination pair. This contains nxn values.
  
  5. Percentile 50 over the packets transmitted for each source-destination pair. This contains nxn values.
  
  6. Percentile 80 over the packets transmitted for each source-destination pair. This contains nxn values.
  
  7. Percentile 90 over the packets transmitted for each source-destination pair. This contains nxn values.
  
  8. Variance of the delay (jitter) over the packets transmitted for each source-destination pair. This contains nxn values.
  
  9. Absolute number of packet losses in each source-destination pair. This contains nxn values
  
  10. Total number of packets lost in the network. This is the sum of all the losses in the previous vector

* tfrecords/train: This is the input TFRecords file that contains the samples used to train RouteNet. To generate this training set, we randomly selected 80% of the samples from the original dataset.

* tfrecords/evaluate: This is the TFRecords file used to evaluate RouteNet. It contains a collection of 20% of the samples extracted randomly from the original dataset.

Datasets can be downloaded from these links:

* NSFNet topology: http://knowledgedefinednetworking.org/data/datasets_v0/nsfnet.tar.gz
<p align="center"> 
  <img src="/assets/nsfnet_topology.png" width="400" alt>
</p>  
* GEANT2 topology: http://knowledgedefinednetworking.org/data/datasets_v0/geant2.tar.gz
<p align="center"> 
  <img src="/assets/geant2_topology.png" width="400" alt>
</p>
* 50-nodes topology: http://knowledgedefinednetworking.org/data/datasets_v0/synth50.tar.gz
<p align="center"> 
  <img src="/assets/synth50_topology.png" width="500" alt>
</p>  
  

Please, if you have any doubt on how to process the datasets do not hesitate to contact us by sending an email to kdn-contactus <at> knowledgedefinednetworking.org. Also, if you would like to be notified with new dataset releases or discuss anything related to the datasets, you can also subscribe to the mailing list kdn-users<at> knowledgedefinednetworking.org (Link: https://mail.knowledgedefinednetworking.org/cgi-bin/mailman/listinfo/kdn-users).
