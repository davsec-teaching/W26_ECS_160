### Detailed Syllabus

#### Module 1 - Building Components
1. Sequential design patterns (Creational, Structural, Behavioral)
2. Parallel design patterns
3. Reflection and metaprogramming
   1. Java reflection and annotations
   2. Java dynamic proxies
   3. Java annotation processing
   4. Case studies: ORM-style frameworks

#### Module 2 - Composing Systems
1. Microservices architecture
2. Communication models
   1. RPC (REST, gRPC)
   2. Messaging (queues, pub-sub)
3. Event-driven design
   1. Events vs Commands
   2. Kafka as a distributed log
      1. Kafka architecture
      1. Partitioning and Replication
      2. Consistency
      3. Monitoring and scaling
      4. Metadata management (Zookeeper, KRaft)
5. Orchestration and deployment
   1. Kubernetes
      1. Control plane (`kube-apiserver`, `etcd`, `kube-scheduler`, `kube-controller-manager`)
      2. Data plane (`kubelet`, `containerd`, `kube-proxy`, CNI plugins (Calico, Cilium, etc)
   3. FaaS/serverless
      1. OpenFaas components - gateway, function pods, autoscaler
     
#### Module 3 - Validating Systems
1. Unit-testing
1. Property-based Testing
2. Fuzz testing (AFL)
   1. Silent errors and sanitizers
   2. Fuzz testing network applications
4. Advanced topics: symbolic execution, abstract interpretation
