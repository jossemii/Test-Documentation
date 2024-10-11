# CELAUT: A peer-to-peer architecture for software design and distribution

<br>

**Context**

In the 1940s, mathematician John von Neumann, in collaboration with Stanislaw Ulam, introduced the concept of cellular automata, creating models that demonstrated how complex behaviors could emerge from simple rules. Building on this foundation, in 1970, John Horton Conway introduced "The Game of Life," a cellular automaton that became a classic example of how a system could evolve into intricate patterns from basic interactions. These ideas offer valuable insights into how decentralized systems can achieve complexity without central control, which serves as a guiding philosophy for the design of CELAUT.

**Definition**

CELAUT is a set of simple rules for software design and distribution, aiming to create a robust, scalable, and adaptive system through the principles of **decentralization**, **simplicity**, and **determinism**.

**Principles**

1. **Decentralization**: No single point of control or failure, allowing nodes to communicate and coordinate dynamically.
2. **Simplicity**: Minimalistic rules that reduce complexity, ensuring each component can be understood and maintained easily.
3. **Determinism**: Services behave predictably, following defined rules that ensure reproducibility of outcomes across different nodes.

**CELAUT Architecture: Nodes and Services**

CELAUT’s architecture is built around two core elements: **nodes** and **services**. Together, they create a distributed network where each part contributes to the overall functionality, much like the agents in a cellular automaton.

- **Nodes**: 
    A node represents a computer or device within the CELAUT network that can communicate with other nodes and manage the execution of services. The decentralized nature of CELAUT is embodied in how nodes interact, ensuring no single point of failure or centralized control. Key responsibilities of a node include:
  
    1. **Service Execution**: Nodes manage service instances, deciding whether to run them locally or distribute the load across peer nodes. This ensures optimal resource utilization and performance, similar to how cells in a cellular automaton interact with their neighbors to maintain system balance.
    
    2. **Communication Interface**: A robust interface ensures seamless data exchange between nodes and services, allowing for effective coordination. Notably, nodes do not need to agree on a specific communication protocol beforehand, as the interface abstracts these details, allowing flexibility in interactions. This feature is further explained in detail below.
    
    3. **Address and Token Provisioning**: Nodes facilitate secure interactions by managing communication addresses and authentication tokens, making services easily accessible while maintaining security.
    
    4. **Dependency Management**: Nodes ensure that services have access to their required dependencies. These dependencies, often referred to as "child services," can run on the same or different nodes. This feature enables a parent service to access and coordinate with its child services, ensuring a smooth and efficient service ecosystem, even as services are distributed across the network.

    An implementation example of a node using Python3 and Rust is **Nodo** [here](https://github.com/celaut-project/nodo).

- **Services**:
    A service in CELAUT is a deterministic software container designed to perform a specific task. Following the **black box** principle, services operate independently of the details of the nodes that execute them, focusing solely on their functionality. Key aspects of services include:
  
    - **Execution as Isolated Instances**: When a user requests a service, it is sent to a node, which runs it as an isolated process—either in a container or a virtual machine, depending on the node’s architecture. This abstraction aligns with the principle of **simplicity**, as services do not need to be aware of the execution environment.
    
    - **Deterministic Behavior**: Services follow predefined rules to ensure consistent outcomes. This aligns with CELAUT's emphasis on **determinism**, where given the same inputs, a service will always produce the same output, regardless of which node executes it.
    
    - **Hierarchical Execution**: A unique characteristic of services in CELAUT is their ability to request the execution of other services (child services) through the node, enabling complex workflows. This mirrors the emergent behaviors seen in cellular automata, where simple interactions can lead to more sophisticated patterns.

**Coordinating Incentives: Reputation and Payments**

In decentralized systems like CELAUT, coordination is not just about technical interactions—it also involves aligning the incentives of all participants to ensure that the system functions effectively. CELAUT incorporates two key mechanisms to achieve this alignment:

- **Reputation Systems**: 
    Each node and service can build a reputation based on its history of interactions. Nodes and services are incentivized to maintain high standards of reliability and quality to preserve their reputation. A strong reputation encourages other nodes and users to trust and interact with them, providing a natural incentive to behave fairly and provide quality services. This reputation mechanism creates a form of self-regulation within the network, where participants have a vested interest in maintaining their good standing.

- **Payment Mechanisms**: 
    Beyond reputation, cooperation between nodes and services is also facilitated through payment systems. When a node executes a service or when services interact with each other, compensation can be exchanged for resources used or tasks completed. These payments create a direct incentive for nodes to contribute resources and for services to deliver value, encouraging collaboration and fair exchanges across the network. Payment mechanisms ensure that resource allocation is efficient, allowing nodes and services to operate based on their strengths and capacities.

These mechanisms are described in more detail later, providing the economic framework that ensures the viability of CELAUT’s decentralized approach. Similarly to how two nodes communicate without needing to agree on a specific protocol, these systems operate independently of the core architecture. The rationale behind this separation, and the way it allows flexibility and adaptability in interactions, is further explained below.

**Bridging the Conceptual and Practical**

The design of CELAUT reflects the underlying principles of cellular automata: simple rules at the node and service level lead to a complex, adaptive system. By decentralizing control, simplifying interactions, and ensuring deterministic behavior, CELAUT creates a flexible framework for software distribution and automation. This allows services to focus on their core functions while nodes manage the orchestration and distribution, leading to a system that can adapt and scale as new requirements emerge.

**Real-World Impact of CELAUT**

The architecture of CELAUT enables faster iteration and experimentation, much like running simulations in a controlled environment. This makes it particularly valuable for scenarios where rapid deployment and testing of new methods are crucial. With the capabilities provided by modern computing, CELAUT can leverage these principles to simulate complex systems in ways that were previously unimaginable, offering new ways to improve efficiency and performance in various industries.

This architecture allows services to focus on their functionality, without worrying about the underlying infrastructure. Nodes, for their part, can efficiently manage the execution of instances, without worrying about their usefulness.

<br>


## How a Service is Specified?

The specification of a service in CELAUT consists of three main components:

### Container | *BOX*
The **BOX** component defines the environment in which the service will run, ensuring consistency in how the service is executed across different nodes. Unlike other containerization methods, CELAUT’s BOX does not rely on external images or repositories; instead, it directly specifies the entire file structure needed to execute the service. It includes the following details:

- **Architecture**: Specifies the microarchitecture of the hardware that the service is intended to run on, ensuring compatibility between the service and the executing node.

- **Filesystem**: The filesystem is a comprehensive description of the service's file structure, encapsulating all the files and directories needed for the service to operate. This includes binaries, libraries, configuration files, and any other necessary resources. 
    - **Item Branches** define the structure of the filesystem, with each branch representing a file, a symbolic link, or a nested directory structure. 
    - This approach allows the entire environment to be self-contained, reducing reliance on third-party repositories and maintaining **determinism**, as the service's execution environment remains the same regardless of the host node.

- **Environment Variables**: Specifies key-value pairs that the service can access during runtime, allowing dynamic configuration without altering the core file structure.

- **Entrypoint**: Defines the script or command that initiates the service's main process when executed. This ensures the node knows how to start the service properly.

- **Config**: This includes configuration paths and formats that the node should load when starting the service. It provides necessary information like initial resource allocations or specific runtime parameters.

- **Expected Gateway**: Describes how the service communicates with the CELAUT node, specifying the protocols and methods (gateway app protocol) it expects from the node. This component ensures smooth communication between the service and its host environment, treating the node like an operating system that provides system-level interactions.

The BOX specification allows CELAUT services to be portable, reproducible, and free from third-party dependencies, aligning with the principles of **simplicity** and **determinism**.

### Interface | *API*
The **API** component defines how clients and other services can interact with a service, specifying the protocols, endpoints, and methods through which data is exchanged. It includes the following elements:

- **App Protocol (AppDef)**: This specifies the application-level protocol that defines the service’s callable functions:
    - **Methods**: Each method has a defined input and output structure, allowing external services or clients to interact with specific functions of the service. This is akin to defining functions in a programming API.
    - For example, a method might allow a client to request data, submit a transaction, or perform a specific computation through the defined slots.

- **Slots**: Slots define the specific functions or methods exposed by the service, associated with network ports and transport protocols. Each slot includes:
    - **Port**: The network port that the service listens on for incoming requests.
    - **Transport Protocol**: The specific protocol used for communication (e.g., TCP, custom binary protocols).

- **Contract Ledger**: Specifies the use of decentralized ledgers for handling payment and reputation. It includes:
    - **Payment Contracts**: Defines how the service handles financial transactions, potentially charging users for its functionality.
    - **Reputation Proofs**: Describes how the service uses reputation contracts, which can be used to verify its reliability and trustworthiness.

- **Cost Function (Optional)**: Defines the conditions under which a service requires payment for its operations. This function is useful for monetizing resource-intensive functions or usage, ensuring transparency in how charges are calculated.

The API specification allows services to be easily accessed and used by clients while maintaining a consistent method of interaction. It supports the **decentralization** principle of CELAUT by allowing services to be self-sufficient in their communication without needing to rely on a centralized controller for protocol negotiation.

### Network | *NET*
The **NET** component defines the scope of external network access that a service can request and interact with. By default, a service is isolated from external networks, only able to communicate with its parent service (the client that created it), its child services, and the CELAUT node that executes it. This isolation ensures **determinism** and enhances **security** by preventing unauthorized data leaks or interactions.

However, some services require access to external networks for their functionality. For example, a service that acts as a Bitcoin node needs to interact with the broader Bitcoin network. To enable this without compromising security, CELAUT allows the following:

- **Controlled External Access**: The service does not directly access external IP addresses or nodes. Instead, it sends a request to its CELAUT node, specifying its need for access to a particular network (e.g., "bitcoin-mainnet"). The CELAUT node then verifies and provides a list of trusted peer nodes that the service can interact with.
  
- **Service Request for Network Peers**: A service like a Bitcoin node may request additional resources or network peers by communicating with its CELAUT node. For example, it might request, "I need peers from 'bitcoin-mainnet'." The node evaluates this request and returns a list of verified instances (which may be other Bitcoin nodes running as services on the CELAUT network).

- **Node Awareness and Redirection**: If the CELAUT node that received the service request knows it cannot find suitable peers (e.g., no other Bitcoin nodes in its network), it will look for a peer CELAUT node that can fulfill this requirement. This ensures that services can always find the necessary network connections, even if the initial CELAUT node is limited.

The **NET** component allows CELAUT to balance the need for external connectivity with the core values of **security** and **determinism**, ensuring that services remain isolated unless explicitly permitted to access broader networks.

<br><br>

The specification of a service is a key part of the CELAUT architecture, as it allows services to be deployed and executed in a consistent and predictable manner. 

<br>

The node will load the service from the binary and provide it with the resources it needs to run.

There is no single way to define a service.
For example, [Proto3 implementation](https://github.com/celaut-project/service-lib/blob/master/node-driver/src/node_driver/gateway/protos/celaut.proto#L66) is one of many possible variations.

<br>

Not all nodes will accept all possible variations of a service specification.

>For example, node A understands a specific proto3 specification and one in JSON. Another node B understands the JSON specification and another in JSON+zip file system. Both nodes can transmit services of the specification they have in common, in this case, JSON.

<br>

## Different types of role users on the system

As users, we can play three types of roles:

- Node maintainer (similar to someone maintaining a miner in a blockchain).
- Service developer. These can be run by anyone on any node (any compatible node, in terms of architecture, etc.).
- Users who launch services on nodes.

Therefore, the person maintaining a node (type 1 user) doesn’t concern themselves with whether it’s mining PoW, running a trading bot, analyzing a DNA sequence, or whatever the services it runs do. They simply execute the services that type 3 users request, in exchange for proof of payment (on a blockchain or whatever method of payment is accepted). The developer (type 2 user) only needs to send it to one or multiple nodes, and these will handle distributing the service among others and/or uploading it to a reputation system,  so that users (or other services) know if use it and when and why to use it.

>This is a simple view of the system, by introducing more complexity the amount of possible user roles could also grow

<br>


## Following Nature's Footsteps in Digital Ecosystems

Imagine CELAUT as a digital ecosystem, mirroring the dynamics of a biological ecosystem found in nature. In this analogy:

1. Nodes as Organisms: Nodes within CELAUT can be likened to organisms in a natural ecosystem. Each node represents a distinct entity with its own capabilities and functions, akin to different species occupying various niches in the environment. These nodes interact with each other, forming a network akin to the interconnected web of life found in ecosystems.

2. Services as Biological Functions: Services within CELAUT are analogous to biological functions or processes found in organisms. Each service performs a specific task, similar to how organs in living organisms carry out specialized functions. Just as organs work together harmoniously to sustain life, services collaborate within nodes to fulfill diverse computational needs.

3. Decentralization as Diversity: The decentralization principle of CELAUT can be equated with biodiversity in natural ecosystems. In nature, biodiversity ensures resilience and adaptability, as diverse species contribute to ecosystem stability and functionality. Similarly, decentralization in CELAUT mitigates risks associated with single points of failure and enhances the system's ability to adapt to changing conditions.

4. Efficiency as Energy Optimization: Efficiency in CELAUT mirrors the energy optimization observed in natural systems. In biological ecosystems, energy flows through food webs, with organisms optimizing energy expenditure to maximize survival and reproduction. Likewise, CELAUT optimizes computational resources, distributing tasks across nodes to minimize latency and resource wastage.

5. Simplicity and Determinism as Natural Laws: The principles of simplicity and determinism in CELAUT resonate with the underlying laws governing natural systems. Just as physical laws dictate the behavior of matter and energy in the universe, CELAUT's simple rules govern the interactions between nodes and services. This deterministic framework ensures consistency and predictability, analogous to the predictability of natural phenomena governed by fundamental laws.

<br>


## Trust systems

In CELAUT, the different parts of the system, nodes and services, do not trust each other, therefore it is a trustless system. This is why it is unlikely that a node will execute services for free, or that a service will not work without making a payment in a contract and receiving proof of it (although obviously they can do it if they want, in the case of services it is economically feasible because they have a marginal cost of zero). 
<br>However, to allow interaction between these parts without trust between them, contracts, social contracts (in a society of nodes and services), are needed to transmit value and assign reputation to each part. Therefore, we have two types of systems (from a relatively abstract point of view): payment systems and reputation systems.

> A possible strategy for a node is to offer the execution of services without exchanging value at the beginning, to increase its reputation and, when it has reputation from others, start to increase its cost.

> Unlike nodes, the nature of services is to have a marginal cost of zero, this means that they do not have a limit on the number of simultaneous units in which they are executed (since the cost of executing them falls on the nodes), so it is quite likely that many services will start with a cost of zero to gain reputation, require a cost when they have reputation and remain competitive, and return to a cost of zero when they cease to be competitive.


### Payment systems

Payment systems allow for the transfer of value between entities in CELAUT. Here are some possible types:

#### License Smart Contracts

A contract system that allows for the issuance of usage licenses for services and nodes, where Ledger is the network where truth is agreed upon. In this way, if A wants to execute a method of B, it will check its contract, execute the defined command (connecting to the Ledger), and the Ledger's contract will issue a license, which A will send to B to enable it to execute the desired method.

There are four distinct types of licenses based on two different classifications. On one hand, whether the license is elastic or static, and on the other, whether it is interactive or non-interactive.

- **Elastic licenses** are those that allow for the restriction of their usage based on certain parameters (number of requests, time, methods, environment variables, etc.).

- **Static licenses** are those that do not restrict usage. B knows the license keys, and the contract provides the license without being able to limit its usage.

- **Interactive licenses** require B to connect to the Ledger to verify the validity of the provided license.

- **Non-interactive** licenses do not require B to connect to the Ledger to verify the validity of the provided license.

<br>

This results in the four types of licenses:

- Interactive static (Very straightforward - Not very useful)
- Non-interactive static (Better for services)
- Interactive elastic (Better for nodes)
- Non-interactive elastic (Quite complex - Versatile)

<br>

> Ledgers can be public and permissionless networks like Bitcoin or Ergo, or private closed platforms like Stripe. The only requirement is that all participants must support them.

<br>


### Reputation systems

Reputation systems allow users, nodes, and services to create a social ecosystem upon which to make decisions. For nodes, they need to know which peers they can trust to request the execution of services. 
For users, who execute services, it helps them determine which services will perform best for the task they wish to carry out.

In Celaut, reputation is represented as records on Ledgers, which represent an opinion.

In the case of services, their deterministic nature provides a different perspective on their reputation compared to nodes. 
A reputation proof (a record) published some time ago may hold the same value as a current one regarding a service (this is true when the service does not interact with networks, which is the default form of a service, completely isolated). 
If it does interact with any network, its reputation may depend on the reputation of the networks it connects to, which does not have that deterministic property, as it can change over time. This is because the service itself has not changed.

However, the reputation of a node is more valuable the more recent it is, as its behavior can vary over time. 
When nodes present themselves to each other, they show proofs of their reputation, and others may opine non-consensually on whether they are more or less trustworthy.

Each node, service, or other type of actor in a reputation system trusts various sources to varying degrees, and these sources, in turn, trust different sources, nodes, services, or other entities to varying degrees. Thus, when a certain actor is presented with an unknown entity, they will check the opinions of their trusted sources.

For a more specific understanding of how a reputation system works, you can read: [Sigma Reputation Panel Documentation](https://github.com/reputation-systems/sigma-reputation-panel/blob/master/README.md)

<br>


## Why is this necessary

CELAUT intend to solve is precisely the separation between the “*how to solve a problem*” and the “*where and who solve it*”.

Take, for example, a trading bot.

>Trading bots are automated software programs that execute buy and sell orders in financial markets based on predefined algorithms. They are important as they can operate 24/7, react quickly to market changes, and remove emotional biases, enhancing efficiency and consistency in trading strategies.


<br>

In this context, if you want to use a trading bot right now, You will go to the web and can:

1. Look for a web service that will manage your asset portfolio, which has:
    1. Advantages:
        1. You don’t need to run the infrastructure yourself.
        2. You don’t need to configure anything.
    2. Disadvantages:
        1. You can’t attribute reputation to it because the developer of the web
        service is unable to prove that the system hasn’t changed (for example,
        when a bot has gained a large number of users, they might reduce
        its performance to encourage you to use a newer one).
        2. The developers of the web service can’t assure you that they aren’t
        misusing the data from your requests (in this case, the
        movements of your portfolio).
2. Search for a source code (on GitHub, etc.) that you can run on your PC (or in the cloud) on your own.
    1. Advantages:
        1. It’s deterministic, in the sense that (if it can’t connect to the
        internet) you’ll be sure that its behavior and/or performance won’t change in the future, because the developer can't modify the source that you previously downloaded.
        2. The developer of the service has no control over the data of your requests.
    2. Disadvantages:
        1. You need to possess equipment (infrastructure) capable of running the code.
        2. You have to deal with system configuration issues (which are often
        significant enough for an average user to opt for a web service).

In contra part of this two options, CELAUT allows to take the advangates of the two previous solutions without their disadvantages. There’s why:



- Infrastructure management is unnecessary, as the nodes handle it. There is no need to seek out a cloud provider.

- No configuration is required. The specification service covers how the container is built, its architecture, its network requirements, and its interface. Users do not need to be concerned about any of this.

- Service developers cannot control, modify, or extract data from the service. They do not control the nodes that distribute and run it. However, they may be incentivized to create it.

<br>


## System behavior

This section describes the system behavior, which includes interactions between the parts of the system, 
in order to show the nature of the architecture with greater clarity.

- [Execution of a service](execution_of_a_service.md)
- [Service load balancing](service_balancer.md)
- [Node handshake]()

<br>


## Design patterns

Design patterns are common solutions to common problems in software development. They are a way to share knowledge and experience among developers.


- [Classifier pattern]()
- [Mirror pattern]()
- [Genetic pattern]()

<br>


## Possible Asked Questions

### How do multiple clients make requests to a specific service?

Based on the architecture developed earlier, if a client wants to use a service, it requests the creation of an instance of it on a node. Therefore, if n clients want to use the service, n instances of it will be needed. In the end, an instance of a service, a container, is nothing more than an isolated thread. Therefore, the client/server paradigm is altered when talking about services on a trustless distributed system. Instead of one server serving multiple clients, a client requests the deployment of a service. In terms of performance, there should not be a big difference between one scheme and another, since the centralized server ends up executing n threads for n client requests. In distributed services, these threads are isolated (<span style="color: gray">which can be a negative point regarding shared memory usage, but this is solved with some other patterns</span>) and are distributed among the nodes in the network.

To create a web service that can be connected to a frontend in a browser, a new service is required to handle incoming requests from clients and deploy and use the classification service. This service, as shown in *Figure 118*, can be understood as a bridge between the "traditional web" and a distributed service network.

![cealut_bridge_to_common_web](assets/188c27-cealut_bridge_to_common_web.excalidraw.svg)*__Figure 118__: Celaut service bridge to a common web pattern*

<br>


### Multiple instances cannot share dependencies

The nature of the network creates a hierarchy of service instances distributed across the nodes of the same. 
This means that when an instance (parent) requests an instance of another service (child or dependency), the latter will become dependent on the former, with the parent assuming the right to decide when the child should be stopped and the responsibility for its resource consumption (in case a node tracks the consumption of each instance, it must take into account that of its dependencies). 

In this way, if the parent is stopped, the node where its child is running can stop it. If it were not so, a node would not know which instances are in a 'zombie' state since none of its clients (parent instances) would be responsible.

<br>


### Why include the entire filesystem in the specification of a service?

The specification of a service must define it completely and comprehensively to avoid redundancy among different services. 

This can lead to problems such as the size of the specification, but these problems can be solved in a higher abstraction layer, such as the 
[pee-rpc protocol](https://github.com/pee-rpc-protocol/pee-rpc#using-blocks-buffer-containers)
does.

>Using Dockerfiles to define a service would not be possible to verify the authenticity of the container, since the Dockerfile defines how to build the container and, generally, are download instructions from central repositories, and they can change the result over the time. A node can accept a specification that uses Dockerfiles, but it would not be in line with the principles of the architecture.

<br>


### Hash Algorithm Identification

To avoid collisions when identifying different hash algorithms, they are identified by applying their own algorithm to an empty input, that is, an empty bit string. 

For example:
- **Sha2562-256 utf-8**: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855

- **Sha3-256 uft-8**: a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a

- **Shake-256 utf-8 of 256 bits**: 46b9dd2b0ba88d13233b3feb743eeb243fcd52ea62b81b82b50c27646ed5762f

- **Sha2562-256 utf-8 over Sha256-256 utf-8**: cd372fb85148700fa88095e3492d3f9f5beb43e555e5ff26d95f5a6adc36f8e6

<br>

In this way, the word "*CELAUT*" would be:
- Sha2562-256 utf-8:

    - type: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
    - value: d7f901be75b36a2ccef2fcc61f9c0665af4155ccee1eda8c4afa1a4f994e825c 
    
- Sha3-256 utf-8:
    - type: a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a
    - value: de1a91208f7c6d320cbc50e45952fe40cb18a949f8a4daae0f8428fc60523f09

- Shake-256 utf-8 of 256 bits:
    - type: 46b9dd2b0ba88d13233b3feb743eeb243fcd52ea62b81b82b50c27646ed5762f
    - value: d10ca3f8a2ab79a8b841876780d232770e70a22cbcc8769a0a1a44194702ee7c

<br>

To avoid collisions with similar algorithms 
<span style="color: gray">(or variations of one, is the same)</span> 
double algorithm identification could be used, for example:
- Sha2562-256 utf-8 over Sha256-256 utf-8:

    - type: cd372fb85148700fa88095e3492d3f9f5beb43e555e5ff26d95f5a6adc36f8e6
    - value: 22bcde154d5f214fe34a5e2b862c0b5aee07d04f62b293b749727ce4b2bea9ad 


<br>

In this way, no centralized registry is required to agree on which identifier corresponds to a specific algorithm.

>In the same way as in the case described in the question regarding the file system, a node is not required to use this hash identification format. This solution is presented because it is in line with the CELAUT principles.


Other way to solve the problem could be 
[multihash from Protocol Labs](https://github.com/multiformats/multihash#format).
But it needs to have a consensus to say what algorithms has what identifier codes.


### How does CELAUT's peer-to-peer network architecture relate to orchestrators like Kubernetes or  Apache Mesos ?

The primary distinction lies in the hierarchical structure of both 
[Kubernetes](https://kubernetes.io/docs/concepts/architecture/nodes/)  and 
[Apache Mesos](https://mesos.apache.org/documentation/latest/architecture/)
, where a single master node oversees a collection of subordinate nodes, or minions. 
In contrast, CELAUT employs a decentralized approach, where no individual node holds authority over another, fostering a trustless environment that can support a vast and globally distributed computational network.

CELAUT offers several integration and utilization possibilities with existing systems:

- Evolving them so that the node operates as a master node in these systems and treats the other peers as subordinate minions (even if they are not, meaning compensating them).

- Operating as standalone services within the distributed network, assuming responsibility for load balancing and coordinating other child services in a centralized fashion. This role can prove beneficial for applications requiring centralized control and orchestration.

- Employing them within traditional centralized server environments, recognizing that CELAUT's inherent trustlessness may not be essential when all nodes are under the control of a single organization. In such scenarios, the master node assumes the responsibility of connecting to external nodes via the CELAUT architecture.

<br>
<br>

## Roadmap

This section covers the past and future development of the project by the initial development team (it is updated over time)

[Link to the roadmap](roadmap.md)

<br>


## Links

[Proof of Concept: CELAUT over IPFS, Feb '21](https://discuss.ipfs.tech/t/proof-of-concept-interplanetary-service-system/10245)

[Ergo-forum conversation, Aug '23](https://www.ergoforum.org/t/artifical-economic-intelligence-on-ergo-blockchain/4429/2)

>IPFS or Ergo are not platforms on which the architecture is based, although both are aligned with the CELAUT principles.

<br>
