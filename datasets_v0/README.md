# Data sets

In this repository you can find the links to download data sets for different network topologies. Each topology corresponds to a compressed file with the following structure:

* *.ned file that describes the network topology. We consider that all the links inthe topology have a capacity of 10 kbps.

* routing: The /routing* directory contains the different routing schemes simulated. Each routing file includes a matrix that describes the forwarding of each node: matrix(src node , dst node) = output port of the src node to forward the traffic towards the dst node.

* delays: The /delays* directory contains the results of the simulation. The file names use the following structure : dGlobal_0_<lambda>_<routing_file>.txt, where <lambda> is the traffic intensity and <routing_file> indicates the routing used for the simulation. Each line in these dGlobal_0_*.txt files corresponds to a simulation with different traffic matrices probabilistically generated from a given traffic intensity (lambda). We describe below the data structure in each line (i.e., each simulation). Note that in a topology with ‘n’ nodes, nodes are enumerated in the range [0, n-1].
  
  1.Bandwidth (in kbps) generated for each source-destination pair in the network. This is a flattened vector with nxn fields, where the index for a particular src-dst pair is:
  
      index=srcnode∗n+dstnode
      
  For example, the position of the bw generated for the pair 0-7 in a network with 10 nodes would be index = 0*10+7 = 7.
  
  2.Average delay over the packets transmitted for each source-destination pair in the network. This contains nxn values. This vector and the subsequent have the same indexes for nodes than the previous vector.
  
  3.Percentile 10 over the packets transmitted for each source-destination pair.  This contains nxn values.
  
  4.Percentile 20 over the packets transmitted for each source-destination pair. This contains nxn values.
  
  5.Percentile 50 over the packets transmitted for each source-destination pair. This contains nxn values.
  
  6.Percentile 80 over the packets transmitted for each source-destination pair. This contains nxn values.
  
  7.Percentile 90 over the packets transmitted for each source-destination pair. This contains nxn values.
  
  8.Variance of the delay (jitter) over the packets transmitted for each source-destination pair. This contains nxn values.
  
  9.Absolute number of packet losses in each source-destination pair. This contains nxn values
  
  10. Total number of packets lost in the network. This is the sum of all the losses in the previous vector

* tfrecords/train: Contains the training subset of the pre processed data. The proportion of training and evaluation is 80% for training and 20% for evaluation, the files in the train folder are the input data of RouteNet from which the model will be trained from.

* tfrecords/evaluate: Contains the evaluation subset of the pre processed data, they are also input data for RouteNet. The evaluation files will be used to evaluate the performance of RouteNet on every train step.

Datasets can be downloaded from these links:

* NSFNet topology: http://knowledgedefinednetworking.org/data/datasets_v0/nsfnet.tar.gz
<p align="center"> 
  <img src="/assets/nsfnet_topology.png" width="400" alt>
</p>  
* GEANT2 topology: http://knowledgedefinednetworking.org/data/datasets_v0/geant2.tar.gz
<p align="center"> 
  <img src="/assets/geant2_topology.png" width="400" alt>
</p>
* Synth50 topology: http://knowledgedefinednetworking.org/data/datasets_v0/synth50.tar.gz
<p align="center"> 
  <img src="/assets/synth50_topology.png" width="500" alt>
</p>  
  

If you have any doubts, do not hesitate to contact us by sending an email to kdn-contactus <at> knowledgedefinednetworking.org. Also, if you would like to be notified with new data set releases or discussanything related to the data sets, you can also subscribe to the mailing list kdn-users<at> knowledgedefinednetworking.org (Link: https://mail.knowledgedefinednetworking.org/cgi-bin/mailman/listinfo/kdn-users). 

