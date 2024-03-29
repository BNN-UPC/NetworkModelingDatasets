# Network Modeling Datasets

Network modeling is essential to build efficient network operation and optimization solutions with special attention on future self-driving networks. One fundamental characteristic of network optimization tools is that they can only optimize what they can model. For example, to optimize the end-to-end delay of traffic, it is necessary a network model able to understand how delay is related to other network characteristics (e.g., topology, configuration, traffic). In this context, nowadays network operators lack efficient network models able to make accurate predictions of relevant end-to-end Key Performance Indicators (KPI) such as delay or jitter at limited cost. On the one hand, analytic models (e.g., Queuing Theory) fail to achieve accurate estimation in real-world scenarios with complex configurations (e.g., real traffic distributions, multi-hop routing). On the other hand, packet-level simulators produce accurate KPI predictions at the expense of high computational cost, which makes them useless for network operation in short timescales.

In the context of Machine Learning (ML), Neural Network (NN) models seem to be promising to build lightweight network models with sufficient accuracy. However, early ML-based attempts did not fulfilled yet the high expectations. We claim that one reason behind the slow progress of ML-based solutions for network modeling is the lack of public datatsets. Datasets are necessary to train and validate ML models, and generating datasets with accurate end-to-end performance metrics (e.g., delay, jitter, drops) in realistic network scenarios is often too expensive given the necessity to make simulations at the packet-level granularity (i.e., packet-level network simulators). Moreover, we need public datasets in order to benchmark new solutions. Without a public dataset comparison is very difficult since each work is evaluated with different data. 

In this repository, we provide datasets with samples generated with a custom-built simulator in OMNet++. Our datasets include samples with different input topologies, routing configurations and traffic patterns, and each sample contains accurate measurements of relevant end-to-end key performance metrics resulting from the simulation. Particularly, they contain statistics of the per-source/destination pair packet-level delay, jitter and losses. Further details can be consulted on the instructions provided in the root directory of every dataset. We hope this will encourage researchers to design, train and evaluate new ML-based network models as well as benchmark them to compare their performance against previous state-of-the-art proposals evaluated over the same dataset.

These datasets have been created using BNNetSimulator. The simulator is now available to the community as a docker image. More information about it can be found in [this repository](https://github.com/BNN-UPC/BNNetSimulator).

Contributors of the datasets:
* Albert López Brescó (Main developer)
* José Suárez-Varela
* Miquel Ferriol-Galmés
* Albert Cabellos-Aparicio
* Pere Barlet-Ros
