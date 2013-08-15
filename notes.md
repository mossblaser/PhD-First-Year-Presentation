First Year Presentation
=======================

Outline
-------

0:00-2:00
`````````
* Brain very complex and attempts to understand it often result in models being
  simulated.
  
  * Spaun is currently the largest 'functional' model built and exhibits
    high-level behaviour such as memory, etc. and respond similarly to
    real-brains when neurons are killed off.
  
  * Sadly they run very slowly: 2.5 hours 
  
  * Artificial neural networks work like this:
    
    * Graph: Human: 85 Billion nodes, ~1000 edges per node
    
    * Nodes receive spikes which accumulate charge and eventually produce a
      spike which is transmitted to neighbours.
    
    * Neuron models are typically pretty simple for large networks
    
    * Average firing rates on the order of 1Hz meaning 1000*85000000000 spikes
      per second, i.e. vast amounts of communication despite small computation
  
  * All this communication relies on having a well suited interconnection
    network connecting computational elements in a simulation machine. My
    project hopes to develop a new interconnection network for neural simulation
    with a focus on interconnect.
  
* Why do conventional machines struggle?
  
  * Big super computers have lots of computation but little communication
    compared to the amount needed for all those spikes.
  
  * Some have tried to make use of this, e.g. Blue Brain with detailed neurons
    but limited to small graphs of 100,000 nodes (so less communication). I'm
    not looking at this.

2:00-4:00
`````````
* What do current brain simulators look like?
  
  * Lets look at three: Neurogrid, BrainScaleS and SpiNNaker, the last of which
    is the focus of my work so far.
  
  * Neurogrid has a chip to simulate 66,048 neurons which produce spikes.
    
    * Each neuron very cheap to simulate as they're simple to implement
      using unconventional analogue electronics and use very little power.
    
    * Spikes are sent off-chip for routing (using conventional digital circuits)
      to other locations using a lookup table.
    
    * Chips are combined into a tree. Trees work nicely because neurons
      connecting to near-by neurons (as they often do) need only traverse a
      small bit of tree: as the system grows, hops in network grows only
      $O(\log{N})$. Also means that spikes between neurons in one place don't
      compete for resources with neurons in another. Usually.
    
    * Sadly, spikes from/to a chip are sent/received serially which wastes a lot
      of potential parallelism, especially considering fanout.
    
    * Trees, while offering some locality, don't match the fact that neurons in
      the brain are really arranged on a sheet so networks with more sheet-like
      properties fit better [Vainbrand & Ginosar] as we see next
  
  * BrainScaleS use a whole silicon wafer
    
    * Still use analogue neuron models on a chip. Chips are made, many to a
      wafer and then chopped up. BrainScaleS doesn't chop them up and uses the
      whole wafer.
    
    * There is an on-wafer mesh network which looks like this (picture). Can
      get around like this (dimension order routing) and matches nicely with the
      brain's 2D sheet structure -- found optimal amongst simple topologies by
      [Vainbrand & Ginosar]
    
    * Off-wafer it gets tricky to scale. Can't really make a mesh any more
      (wafers are circular).
  
  * SpiNNaker uses small but general purpose digital processor cores (rather
    than specialised hardware) which gives flexibility.
    
    * Have 18-core chips which can connect to neighbours.
    
    * Chips are arranged in a toroid which is like a torus (show turning a mesh
      into a torus) but with more links (show spinnaker topology). Fits the
      brain nicely as with a mesh.
      
    * Scalability only limited by addressing (can just keep adding chips)
      
    * Notably this machine also is readily available: work has focused mainly on
      this machine.

4:00-8:00
`````````
* Preliminary work has focused on understanding and extending the
  interconnection network of SpiNNaker.
  
  * Simulation of interconnect
  
    * Not all connections in a network are the same. SpiNNaker has used
      different techs between chips sharing a circuit board and those on
      different boards.
    
    * Built a simulator to test the performance of the network and found that
      latency increases by 80%. Not a problem at present but may become so.
  
  * Wiring practicalities of a real machine
    
    * For the largest planned SpiNNaker machine there will be 1,200 boards each
      connected together using 3,600 wires.
    
    * Wires must be cheap, short and easy to wire up
    
    * Systems typically live in cabinets and racks which complicates wiring.
    
      * Means things are forced into an approximately 2D arrangement: a toroid
        will have long wires which require folding to avoid.
        
      * The 2D cabinet arrangement doesn't match the network's
        dimensions complicates wiring greatly.
      
    * Tool was built for experimenting with layouts and a layout was developed
      (juicy picture). The wiring looks tough but actually regularity can be
      found: 53 patterns needed only.
    
    * This is a challenge for many topologies and this tool may help in finding
      practical ways of assembling complex topologies.
  
  * Small-World Interconnect
    
    * Real world networks such as the network of neurons in the brain or, more
      famously, social networks exhibit the `small world' property. Lots of folk
      but not many hops between them. Watts/Strogatz have an algorithm for
      making them.
      
    * Jist is that adding a small number of random connections to a regular
      network can drop the number of hops required from A to B (and thus the
      latency)
    
    * Though others have tried this, I've done it with respect to wiring
      considerations. Surprisingly this still results in improvements.

8:00-10:00
``````````
* Research plan/Conclusion
  
  * Build/extend simulator as part of work on SpiNNaker project.
  
  * Use simulator to test performance of small-world-style networks described
    previously under real conditions.
  
  * Experiment with other less common topologies, including express cubes, small
    world, overlaid tree/mesh etc.
  
  * Place and routability analysis for tested topologies: P&R are usually
    NP-complete! Need to be able to easily exploit a topology using
    heuristic-based P&R to get performance. 
    
  * Multicast is important, extend simulations again!
  
  * Interconnect technologies are important (see preliminary work), revisit this
    theme by looking at best tech for a new arch. Plenty of variables to
    consider, lots of technologies though HSS very common.
    
  * Arch design and evaluation.
  
  * Write-up. Long time given to allow revisiting things as required.
