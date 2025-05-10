# Comprehensive Kubernetes Revision Notes

## Introduction to Kubernetes

Kubernetes (often shortened to K8s, as there are 8 letters between 'K' and 's') is an open-source platform designed to automate deploying, scaling, and managing containerized applications. It was originally developed by Google based on their internal system called Borg and is now maintained by the Cloud Native Computing Foundation (CNCF).

### What is Kubernetes?

Kubernetes provides a container-centric management environment. It orchestrates computing, networking, and storage infrastructure on behalf of user workloads. This provides much of the simplicity of Platform as a Service (PaaS) with the flexibility of Infrastructure as a Service (IaaS), enabling portability across infrastructure providers.

Think of Kubernetes as an operating system for your cluster. Just as a computer operating system manages how applications use resources on a single computer, Kubernetes manages how containerized applications use resources across multiple machines.

### Why Kubernetes?

- **Portability**: Run applications consistently across on-premises, public, private, or hybrid clouds
- **Scalability**: Scale applications up and down with simple commands, via UI, or automatically based on CPU usage
- **Self-healing**: Automatically restarts containers that fail, replaces and reschedules containers when nodes die
- **Service discovery and load balancing**: No need to modify your application to use an unfamiliar service discovery mechanism
- **Automated rollouts and rollbacks**: Kubernetes progressively rolls out changes to your application or its configuration, while monitoring application health
- **Secret and configuration management**: Deploy and update secrets and application configuration without rebuilding your image
- **Storage orchestration**: Automatically mount a storage system of your choice, whether from local storage, public cloud providers, or network storage systems
- **Batch execution**: In addition to services, Kubernetes can manage your batch and CI workloads, replacing containers that fail

### Kubernetes vs. Docker

A common source of confusion: Kubernetes and Docker are not directly comparable, as they serve different purposes:

- **Docker** is a containerization platform that allows you to package your application and its dependencies into a standardized unit (a container)
- **Kubernetes** is a container orchestration platform that helps you manage and deploy these containers at scale

In practice, Kubernetes can use Docker as its container runtime, but it can also use alternatives like containerd or CRI-O.

## Core Concepts in Depth

### Containers

Containers are lightweight, standalone, executable packages that include everything needed to run a piece of software:
- The application code
- Runtime environment (e.g., JVM for Java applications)
- System tools and libraries
- Configuration files and settings

#### Container Benefits:

1. **Isolation**: Containers isolate applications from one another and from the underlying infrastructure
2. **Portability**: "Build once, run anywhere" - containers run the same regardless of environment
3. **Efficiency**: Containers share the host OS kernel, making them more lightweight than VMs
4. **Speed**: Containers start up in seconds rather than minutes (unlike VMs)
5. **Density**: You can run many more containers than VMs on the same hardware
6. **Immutability**: Container images are immutable, ensuring consistency across environments

#### Container Implementation:

Containers leverage two key Linux kernel features:
- **Namespaces**: Provide isolation for processes, making them see only specific resources
- **Control Groups (cgroups)**: Limit and isolate resource usage (CPU, memory, disk I/O, etc.)

Unlike virtual machines, containers don't run a separate OS instance. They all share the host's kernel but run in isolated user spaces.

### Pods

Pods are the smallest and most basic deployable units in Kubernetes. A pod represents a single instance of a running process in your cluster.

#### Key Characteristics of Pods:

1. **Shared Context**: Containers in a pod share:
   - Network namespace (they can talk to each other using localhost)
   - IPC namespace (they can communicate using inter-process communication)
   - UTS namespace (they share a hostname)
   - Storage volumes

2. **Atomic Unit**: Kubernetes always treats the containers in a pod as a single unit. They are:
   - Always scheduled together on the same node
   - Started together
   - Terminated together

3. **Pod Lifecycle**:
   - **Pending**: Pod has been accepted but containers are not yet running
   - **Running**: At least one container is running
   - **Succeeded**: All containers have terminated successfully
   - **Failed**: At least one container has terminated with failure
   - **Unknown**: State of the pod couldn't be determined

4. **Pod IP**: Each pod gets its own IP address. This means:
   - Containers within a pod can find each other via localhost
   - Containers in different pods must use the pod's IP to communicate

#### When to Use Multi-Container Pods:

Multi-container pods should represent a single cohesive application unit. Common patterns include:

1. **Sidecar Pattern**: Enhances the main container (e.g., logging agent, monitoring agent)
   ```
   Main Container <--> Sidecar Container
   (Application)     (Log Collector)
   ```

2. **Ambassador Pattern**: Proxy communication to and from the main container
   ```
   Main Container <--> Ambassador Container <--> External Services
   (Application)     (API Proxy)
   ```

3. **Adapter Pattern**: Standardizes and normalizes output of the main container
   ```
   Main Container <--> Adapter Container <--> Monitoring System
   (Application)     (Format Adapter)
   ```

#### Pod Definition Example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: myapp
    tier: frontend
spec:
  containers:
  - name: main-app
    image: nginx:1.14.2
    ports:
    - containerPort: 80
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
  - name: log-collector
    image: fluentd:v1.4
    volumeMounts:
    - name: log-storage
      mountPath: /var/log
  volumes:
  - name: log-storage
    emptyDir: {}
```

### Nodes

Nodes are the worker machines in Kubernetes where your containers actually run. A node can be a physical machine or a virtual machine.

#### Node Components:

1. **Kubelet**: The primary node agent that:
   - Ensures containers described in PodSpecs are running and healthy
   - Communicates with the control plane using the Kubernetes API
   - Manages the containers' lifecycle
   - Reports on the health and status of the node and pods

2. **Container Runtime**: The software responsible for running containers:
   - Docker
   - containerd
   - CRI-O
   - Any implementation of the Kubernetes Container Runtime Interface (CRI)

3. **Kube-proxy**: A network proxy that:
   - Maintains network rules on nodes
   - Implements part of the Kubernetes Service concept
   - Handles network communication inside or outside the cluster
   - Uses the operating system packet filtering layer or forwards the traffic itself

#### Node Status:

- **Addresses**: HostName, InternalIP, ExternalIP
- **Conditions**: OutOfDisk, Ready, MemoryPressure, DiskPressure, NetworkUnavailable, etc.
- **Capacity**: Available resources (CPU, memory, pods)
- **Info**: Kernel version, OS, container runtime, etc.

#### Node Management:

1. **Node Registration**: 
   - Self-Registration: The kubelet can register itself with the API server
   - Manual Registration: Admins can create Node objects manually

2. **Node Maintenance**:
   - Marking nodes unschedulable before maintenance: `kubectl cordon <node-name>`
   - Evacuating a node for maintenance: `kubectl drain <node-name>`

3. **Node Monitoring**:
   - The control plane monitors node health and takes action when nodes fail
   - The Node Problem Detector can be used to monitor node health

### Cluster Architecture

A Kubernetes cluster consists of a set of nodes that run containerized applications. The cluster allows containers to run across multiple machines and environments.

#### Components:

1. **Control Plane** (Master Nodes):
   - API Server
   - etcd
   - Scheduler
   - Controller Manager
   - Cloud Controller Manager

2. **Worker Nodes**:
   - Kubelet
   - Container Runtime
   - Kube-proxy

#### Cluster Topologies:

1. **Single Control Plane Cluster**:
   - One control plane node
   - Multiple worker nodes
   - Not suitable for production (single point of failure)

2. **High Availability (HA) Cluster**:
   - Multiple control plane nodes
   - Multiple worker nodes
   - etcd either stacked on control plane nodes or as an external cluster
   - Load balancer in front of API servers

3. **Regional/Multi-zone Clusters**:
   - Nodes distributed across multiple availability zones
   - Provides resilience against zone failures

4. **Multi-cluster Deployments**:
   - Multiple independent Kubernetes clusters
   - Connected using multi-cluster service discovery and federation

#### Communication Patterns:

1. **Node-to-Control Plane**:
   - Kubelets communicate with API server
   - Secure communication using TLS
   - Kubelets watch for changes to their assigned pods

2. **Control Plane-to-Node**:
   - API server initiates connections to kubelet, nodes, pods
   - For operations like exec, logs, port-forward
   - Secure communication using TLS

3. **API Server Communication**:
   - Everything communicates through the API server
   - No direct communication between components
   - Enforces authentication, authorization, admission control

## Control Plane Components in Detail

The Control Plane is responsible for global decisions about the cluster (for example, scheduling), as well as detecting and responding to cluster events.

### API Server (kube-apiserver)

The API Server is the front door to the Kubernetes control plane, exposing the Kubernetes API.

#### Key Functions:

1. **Authentication**: Verifies who is sending the request
   - Client certificates, bearer tokens, authentication proxy
   - OpenID Connect integration (e.g., with enterprise identity systems)

2. **Authorization**: Verifies what the authenticated user is allowed to do
   - RBAC (Role-Based Access Control) 
   - ABAC (Attribute-Based Access Control)
   - Node Authorization
   - Webhook Authorization

3. **Admission Control**: Intercepts requests after authorization but before persistence
   - Mutating admission controllers can modify requests
   - Validating admission controllers can validate requests
   - Examples: NamespaceLifecycle, LimitRanger, ServiceAccount

4. **Data Validation**: Validates and processes API objects
   - Converts JSON/YAML to internal format
   - Validates against schemas
   - Persists objects to etcd

5. **API Versioning and Extensibility**:
   - Supports multiple API versions (v1, v1beta1, etc.)
   - Supports Custom Resource Definitions (CRDs)
   - Aggregation Layer for more complex API extensions

#### Horizontal Scaling:

The API server is designed to scale horizontally. You can run multiple instances behind a load balancer.

### etcd

etcd is a distributed, reliable key-value store used as Kubernetes' backing store for all cluster data.

#### Key Characteristics:

1. **Consistency**: etcd is strongly consistent - reads will always show the most recent writes
2. **Availability**: etcd can tolerate machine failures
3. **Persistence**: etcd persists data to disk
4. **Security**: Supports TLS client certificates for authentication
5. **Performance**: Optimized for fast reads and occasional writes

#### etcd Configuration Options:

1. **Stacked Topology**: etcd running on control plane nodes
   - Simpler to set up
   - Less infrastructure to manage
   - Higher risk - failure affects both etcd and control plane

2. **External etcd Topology**: etcd running on separate machines
   - Better isolation
   - Less risk of resources contention
   - More infrastructure to manage

3. **Cluster Size**: 
   - For high availability: minimum 3 nodes (can tolerate 1 failure)
   - Recommended for production: 5 nodes (can tolerate 2 failures)
   - Maximum recommended: 7 nodes (diminishing returns after this)

#### etcd Backup and Disaster Recovery:

1. **Backup Methods**:
   - Snapshot using etcdctl: `etcdctl snapshot save`
   - Volume backup

2. **Restore Methods**:
   - From snapshot: `etcdctl snapshot restore`
   - Creating a new cluster from backup

3. **Best Practices**:
   - Regular, scheduled backups
   - Testing restore procedures
   - Storing backups off-site

### Scheduler (kube-scheduler)

The Scheduler watches for newly created pods with no assigned node and selects a node for them to run on.

#### Scheduling Process:

1. **Filtering**: Finds nodes that are feasible for the pod (meet basic requirements)
   - Resource requirements (CPU, memory)
   - Port availability
   - Node selector/affinity
   - Taints and tolerations
   - Volume restrictions

2. **Scoring**: Ranks the remaining nodes by "desirability"
   - Available resources
   - Current resource usage
   - Pods already running (spreading vs. packing)
   - Inter-pod affinity/anti-affinity
   - Data locality

3. **Binding**: Assigns the pod to the highest-scoring node

#### Advanced Scheduling Concepts:

1. **Node Affinity/Anti-Affinity**: Rules for which nodes pods should or should not be scheduled on
   ```yaml
   nodeAffinity:
     requiredDuringSchedulingIgnoredDuringExecution:
       nodeSelectorTerms:
       - matchExpressions:
         - key: kubernetes.io/e2e-az-name
           operator: In
           values:
           - e2e-az1
           - e2e-az2
   ```

2. **Pod Affinity/Anti-Affinity**: Rules for which pods should or should not be co-located
   ```yaml
   podAffinity:
     requiredDuringSchedulingIgnoredDuringExecution:
     - labelSelector:
         matchExpressions:
         - key: security
           operator: In
           values:
           - S1
       topologyKey: topology.kubernetes.io/zone
   ```

3. **Taints and Tolerations**: Allow nodes to repel pods
   - Taint a node: `kubectl taint nodes node1 key=value:NoSchedule`
   - Add toleration to pod:
     ```yaml
     tolerations:
     - key: "key"
       operator: "Equal"
       value: "value"
       effect: "NoSchedule"
     ```

4. **Custom Schedulers**: Run alongside the default scheduler
   - Handle specific types of pods
   - Implement specialized scheduling logic

### Controller Manager (kube-controller-manager)

The Controller Manager runs controller processes that regulate the state of the system.

#### Core Controllers:

1. **Node Controller**:
   - Monitors node health
   - Responds when nodes become unreachable
   - Removes pods from unreachable nodes
   - Configurable parameters: PodEvictionTimeout, NodeMonitorGracePeriod

2. **Job Controller**:
   - Watches for Job objects
   - Creates pods to run one-time tasks
   - Tracks job completion and manages retries

3. **Endpoints Controller**:
   - Populates the Endpoints object
   - Links Services and Pods
   - Creates endpoint records for service ports

4. **Service Account & Token Controllers**:
   - Create default accounts and API access tokens for new namespaces
   - ServiceAccount controller ensures at least one default ServiceAccount exists in each namespace
   - Token controller adds a token secret to each ServiceAccount

5. **Replication Controller**:
   - Ensures the correct number of pods are running for each ReplicationController object
   - Uses a reconciliation loop to compare desired and actual state

6. **Deployment Controller**:
   - Implements the functionality of Deployment objects
   - Manages ReplicaSets
   - Handles rolling updates and rollbacks

7. **StatefulSet Controller**:
   - Manages StatefulSet objects
   - Ensures stable identities and persistent storage for pods
   - Handles ordered deployment, scaling, and updates

8. **DaemonSet Controller**:
   - Ensures that specific pods run on all or some nodes
   - Adds pods to newly created nodes
   - Removes pods from deleted nodes

#### Controller Pattern:

All controllers follow a common pattern:
1. **Watch**: Monitor the actual state through the API server
2. **Compare**: Check if the actual state matches the desired state
3. **Act**: Take actions to converge toward the desired state
4. **Update**: Update status through the API server

This reconciliation loop is continuous and operates in a non-blocking manner.

### Cloud Controller Manager (cloud-controller-manager)

The Cloud Controller Manager acts as a bridge between Kubernetes components and a cloud provider's API. It allows the core Kubernetes code to evolve independently of cloud-provider-specific code.

#### Cloud-Specific Controllers:

1. **Node Controller**:
   - Checks the cloud provider to determine if a node has been deleted
   - Updates node address information in the cloud

2. **Route Controller**:
   - Sets up routes in the cloud
   - Ensures pod network routing works correctly

3. **Service Controller**:
   - Creates, updates, and deletes cloud provider load balancers
   - Updates Services with load balancer information

4. **Volume Controller**:
   - Creates, attaches, and mounts volumes
   - Interacts with the cloud storage API

#### Cloud Provider Interface:

Cloud providers can implement a standardized interface to integrate with Kubernetes. Major cloud providers with implementations include:
- Amazon Web Services (AWS)
- Microsoft Azure
- Google Cloud Platform (GCP)
- OpenStack
- VMware
- IBM Cloud
- Alibaba Cloud

## Kubernetes Objects and Workloads

Kubernetes objects are persistent entities that represent the state of your cluster. These objects describe:
- What containerized applications are running
- The resources available to those applications
- The policies around how those applications behave

### Common Fields

Most Kubernetes objects share common metadata:

```yaml
apiVersion: # Which version of the Kubernetes API to use
kind:       # What kind of object this is
metadata:   # Data that helps identify the object
  name:     # Name of the object, unique within a namespace
  namespace: # Optional namespace (defaults to "default")
  labels:   # Optional key/value pairs for organizing objects
  annotations: # Optional key/value pairs for non-identifying info
spec:       # The desired state for the object
```

### Deployments

Deployments represent a set of multiple, identical Pods with no unique identities. They are designed for stateless applications where any instance can replace any other instance.

#### Key Functions:

1. **Declarative Updates**: Define the desired state, Kubernetes handles the rest
2. **Rolling Updates**: Update pods gradually without downtime
3. **Rollbacks**: Easily revert to previous versions
4. **Scaling**: Scale the number of running pods up or down
5. **Pause/Resume**: Pause and resume updates for multiple changes at once

#### Deployment Strategies:

1. **RollingUpdate (default)**:
   - Replace old pods with new ones gradually
   - Control with `maxSurge` and `maxUnavailable` parameters
   ```yaml
   strategy:
     type: RollingUpdate
     rollingUpdate:
       maxSurge: 25%
       maxUnavailable: 25%
   ```

2. **Recreate**:
   - Terminate all old pods before creating new ones
   - Results in downtime but ensures no mixed versions
   ```yaml
   strategy:
     type: Recreate
   ```

3. **Canary Deployments**:
   - Route a small percentage of traffic to new version
   - Implemented using multiple deployments + service

4. **Blue/Green Deployments**:
   - Set up new version alongside old version
   - Switch traffic all at once by updating service
   - Implemented using multiple deployments + service

#### Deployment Example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: "100m"
            memory: "128Mi"
          limits:
            cpu: "200m"
            memory: "256Mi"
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
```

#### Deployment Management Commands:

- Create: `kubectl apply -f deployment.yaml`
- Update: `kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1`
- Roll back: `kubectl rollout undo deployment/nginx-deployment`
- Scale: `kubectl scale deployment/nginx-deployment --replicas=5`
- Pause: `kubectl rollout pause deployment/nginx-deployment`
- Resume: `kubectl rollout resume deployment/nginx-deployment`
- Status: `kubectl rollout status deployment/nginx-deployment`
- History: `kubectl rollout history deployment/nginx-deployment`

### ReplicaSets

ReplicaSets ensure that a specified number of pod replicas are running at any given time. They are the engine behind Deployments.

#### Key Characteristics:

1. **Pod Template**: Defines what each Pod should look like
2. **Selector**: Identifies which Pods it can acquire
3. **Replica Count**: Number of desired replicas

Usually, you don't create ReplicaSets directly but instead create Deployments which manage ReplicaSets for you.

#### ReplicaSet Example:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: gcr.io/google_samples/gb-frontend:v3
```

#### How ReplicaSets Work:

1. **Pod Selection**: ReplicaSets can acquire pre-existing pods using selectors
2. **Pod Creation/Deletion**: Creates or deletes pods to meet desired count
3. **Non-Template Pods**: Can control pods that don't match the template but match the selector
4. **Pod Deletion**: When a ReplicaSet is deleted, its pods can be deleted automatically (set `propagationPolicy: Foreground`)

### StatefulSets

StatefulSets are specialized Deployments for managing stateful applications like databases. They maintain a sticky identity for each pod.

#### Key Features:

1. **Stable Network Identity**:
   - Predictable pod names: `<statefulset-name>-<ordinal>`
   - Stable DNS hostnames: `<pod-name>.<service-name>.<namespace>.svc.cluster.local`

2. **Ordered Deployment and Scaling**:
   - Pods are created in order (0, 1, 2, ...)
   - Scaling down happens in reverse order

3. **Stable Storage**:
   - Ensures each pod gets the same persistent volume across restarts
   - Volume persists even if pod is rescheduled to different node

4. **Ordered, Automated Rolling Updates**:
   - Updates happen one pod at a time, in order
   - Control through update strategies

#### StatefulSet Example:

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

#### Best Practices for StatefulSets:

1. **Headless Service**: Create a headless service (with `clusterIP: None`) for network identity
2. **Persistent Storage**: Use `volumeClaimTemplates` for persistent storage
3. **Pod Management Policy**: Consider `podManagementPolicy: Parallel` for non-dependent pods
4. **Update Strategy**: Set appropriate `updateStrategy` for application needs
5. **Pod Disruption Budget**: Use PDBs to ensure high availability during maintenance

### DaemonSets

DaemonSets ensure that all (or some) nodes run a copy of a pod. As nodes are added to the cluster, pods are added to them. As nodes are removed, those pods are garbage collected.

#### Use Cases:

1. **Cluster Storage Daemons**: e.g., `ceph`, `glusterd`
2. **Log Collectors**: e.g., `fluentd`, `logstash`
3. **Node Monitoring**: e.g., `prometheus node exporter`, `collectd`
4. **Network Plugins**: e.g., `calico`, `weave`, `flannel` agents

#### DaemonSet Example:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-elasticsearch
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: quay.io/fluentd_elasticsearch/fluentd:v2.5.2
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
```

#### Key Features:

1. **Node Selection**: 
   - Run on all nodes by default
   - Can target specific nodes using `nodeSelector`, affinity, or tolerations

2. **Scheduled Pods**:
   - Respect resource requests and limits
   - Respect taints and tolerations
   - Use the Kubernetes scheduler

3. **Updates**:
   - Support rolling updates like Deployments
   - Support rollbacks

### Jobs and CronJobs

#### Jobs

Jobs create one or more pods and ensure that a specified number of them successfully complete.

#### Job Types:

1. **One Shot Jobs**: Single pod running once to completion
2. **Parallel Jobs with Fixed Completion Count**: Multiple pods completing a specific number of tasks
3. **Parallel Jobs with Work Queue**: Multiple pods processing items from a work queue

#### Job Example:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  completions: 5     # The number of pods that need to complete successfully
  parallelism: 2     # How many pods to run in parallel
  backoffLimit: 4    # Number of retries before considering the job failed
  activeDeadlineSeconds: 600  # The job can run for a maximum of 10 minutes
  template:
    spec:
      containers:
      - name: pi
        image: perl
        command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
```

#### Job Patterns:

1. **Task Completion**:
   - Default behavior
   - Creates pods until `completions` number succeed

2. **Queue with Worker Pattern**:
   - Set `parallelism` but not `completions`
   - Workers process items from a queue and exit when done
   - Job completes when all workers finish

3. **Indexed Job**:
   - Each pod gets a completion index (0 to completions-1)
   - Useful for partitioning work by index

#### CronJobs

CronJobs create Jobs on a repeating schedule, like a cron task in Linux.

#### CronJob Example:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"  # Cron format (min hour day-of-month month day-of-week)
  concurrencyPolicy: Forbid  # Allow/Forbid/Replace
  successfulJobsHistoryLimit: 3  # How many successful job executions to keep
  failedJobsHistoryLimit: 1  # How many failed job executions to keep
  startingDeadlineSeconds: 200  # How late a job can start and still be considered valid
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            command:
            - /bin/sh
            - -c
            - date; echo Hello from Kubernetes
          restartPolicy: OnFailure
```

#### Key Features:

1. **Cron Schedule**: Standard cron syntax (minute hour day-of-month month day-of-week)
2. **Concurrency Policy**:
   - `Allow`: Allow concurrent jobs (default)
   - `Forbid`: Skip new job if previous job still running
   - `Replace`: Cancel currently running job and start a new one
3. **Job History**: Control how many successful and failed jobs to keep
4. **Starting Deadline**: Set a limit on how late a job can start

## Services and Networking

### Services

Services provide a way to expose an application running on a set of pods as a network service.

#### Service Types:

1. **ClusterIP (default)**:
   - Exposes the service on an internal IP within the cluster
   - Only reachable from within the cluster

2. **NodePort**:
   - Exposes the service on each node's IP at a static port
   - Accessible from outside the cluster
   - Automatically creates a ClusterIP service

3. **LoadBalancer**:
   - Exposes the service using a cloud provider's load balancer
   - Automatically creates NodePort and ClusterIP services

4. **ExternalName**:
   - Maps the service to a DNS name
   - No proxying, just DNS CNAME record

#### Service Example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - name: http
      protocol: TCP
      port: 80        # Port exposed by the service
      targetPort: 9376 # Port on the pods
  type: ClusterIP
```

#### Service Discovery:

1. **Environment Variables**:
   - For each active service, Kubernetes adds environment variables to pods
   - Format: `{SVCNAME}_SERVICE_HOST` and `{SVCNAME}_SERVICE_PORT`

2. **DNS**:
   - CoreDNS in Kubernetes provides DNS records for services
   - Format: `<service-name>.<namespace>.svc.cluster.local`
   - Creates both A records and SRV records for named ports

#### Service Proxy Modes:

1. **iptables Mode** (default):
   - Purely uses iptables to direct traffic
   - Random selection of backend pod
   - More efficient than userspace mode

2. **IPVS Mode** (for clusters with many services):
   - Uses Linux IPVS for load balancing
   - More efficient with many services and backends
   - Supports more load balancing algorithms

3. **Userspace Mode** (legacy):
   - kube-proxy watches the API server
   - For each service, opens a port on the node
   - Proxies connections to one of the service's pods

#### Advanced Service Features:

1. **Headless Services**: 
   - Set `clusterIP: None`
   - No load balancing or proxying
   - Returns DNS A records for all pods
   - Essential for StatefulSets

2. **ExternalTrafficPolicy**:
   - `Cluster`: Route external traffic to all ready endpoints (default)
   - `Local`: Route external traffic only to local node endpoints

3. **Session Affinity**:
   - `None`: Random distribution (default)
   - `ClientIP`: Stick to the same pod based on client IP

4. **Multi-Port Services**:
   - Expose multiple ports for a single service
   - Each port must have a name for clarity
   ```yaml
   ports:
   - name: http
     port: 80
     targetPort: 8080
   - name: https
     port: 443
     targetPort: 8443
   ```

5. **EndpointSlices**:
   - Provides a more scalable way to track network endpoints
   - Automatically created when you create a Service
   - Distributes endpoints across multiple resources for efficiency

### Ingress

Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

#### Ingress vs. Service:

- **Services** expose applications using a single port on a single IP
- **Ingress** provides external access to multiple services through a single IP address

#### Key Components:

1. **Ingress Resources**: Kubernetes API objects that define rules
2. **Ingress Controllers**: Implementations that satisfy the rules (e.g., nginx, traefik, istio, haproxy)

#### Ingress Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: myapp.example.com      # Optional host name
    http:
      paths:
      - path: /                  # Path to match
        pathType: Prefix         # Exact, Prefix, or ImplementationSpecific
        backend:
          service:
            name: my-service
            port:
              number: 80
  - host: api.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 8080
  tls:                           # Optional TLS configuration
  - hosts:
    - myapp.example.com
    secretName: myapp-tls-secret
```

#### Ingress Features:

1. **Path-based Routing**:
   - Route to different services based on URL paths
   - Example: `/app1 -> service1`, `/app2 -> service2`

2. **Name-based Virtual Hosting**:
   - Different hosts go to different services
   - Example: `myapp.example.com -> service1`, `api.example.com -> service2`

3. **TLS/SSL Termination**:
   - Define TLS secrets for secure connections
   - Certificate handling at the Ingress level

4. **Load Balancing**:
   - Most Ingress controllers provide load balancing
   - Configuration through annotations

5. **Rewrites and Redirects**:
   - URL rewriting (e.g., `/api -> /`)
   - HTTP to HTTPS redirects

#### Ingress Controller Annotations:

Ingress controllers use annotations for controller-specific configurations. Examples for NGINX:

```yaml
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/proxy-body-size: "10m"
    nginx.ingress.kubernetes.io/proxy-connect-timeout: "30"
```

#### Common Ingress Controllers:

1. **NGINX Ingress Controller**: Most popular, developed by Kubernetes community
2. **Traefik**: Modern HTTP reverse proxy and load balancer
3. **HAProxy Ingress**: Based on HAProxy load balancer
4. **Istio Ingress Gateway**: Part of Istio service mesh
5. **AWS ALB Ingress Controller**: Uses AWS Application Load Balancer

### Network Policies

Network Policies specify how groups of pods are allowed to communicate with each other and other network endpoints.

#### Default Behavior:

Without Network Policies:
- All pods can communicate with all other pods (open by default)
- All ingress and egress traffic is allowed

#### Network Policy Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-network-policy
  namespace: default
spec:
  podSelector:            # Which pods this policy applies to
    matchLabels:
      role: db
  policyTypes:            # Specify policy types
  - Ingress
  - Egress
  ingress:                # Incoming traffic rules
  - from:
    - podSelector:        # Allow traffic from pods with label role: frontend
        matchLabels:
          role: frontend
    - ipBlock:            # Allow traffic from specific CIDR
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:  # Allow traffic from pods in namespace with label team: operations
        matchLabels:
          team: operations
    ports:                # Restrict to specific ports
    - protocol: TCP
      port: 3306
  egress:                 # Outgoing traffic rules
  - to:
    - podSelector:        # Allow traffic to pods with label role: monitoring
        matchLabels:
          role: monitoring
    ports:
    - protocol: TCP
      port: 5978
```

#### Key Features:

1. **Selectors**:
   - `podSelector`: Select pods the policy applies to
   - `namespaceSelector`: Select namespaces for ingress/egress rules
   - `ipBlock`: Select IP CIDR ranges

2. **Rule Types**:
   - `ingress`: Rules for incoming traffic
   - `egress`: Rules for outgoing traffic

3. **Port Specification**:
   - Restrict traffic to specific protocols and ports

4. **Default Deny**:
   - Create a default deny-all policy:
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: default-deny-all
   spec:
     podSelector: {}     # Applies to all pods
     policyTypes:
     - Ingress
     - Egress
   ```

#### Common Network Policy Patterns:

1. **Deny all ingress traffic**:
   ```yaml
   spec:
     podSelector: {}
     policyTypes:
     - Ingress
   ```

2. **Allow traffic from specified pods only**:
   ```yaml
   spec:
     podSelector:
       matchLabels:
         app: myapp
     ingress:
     - from:
       - podSelector:
           matchLabels:
             app: allowed-app
   ```

3. **Allow traffic to specific ports only**:
   ```yaml
   spec:
     podSelector:
       matchLabels:
         app: myapp
     ingress:
     - ports:
       - port: 80
         protocol: TCP
   ```

4. **Allow external traffic to specific service**:
   ```yaml
   spec:
     podSelector:
       matchLabels:
         app: web
     ingress:
     - from: []  # Empty array means all sources
       ports:
       - port: 80
   ```

### DNS and Service Discovery

Kubernetes provides a DNS service for service discovery within the cluster.

#### How it Works:

1. **CoreDNS**: The default DNS server in Kubernetes
2. **DNS Records**: Created automatically for Services and Pods
3. **Pod DNS Resolution**: Configured via kubelet and resolv.conf

#### DNS Records Created:

1. **Services**:
   - Regular Service: `<service-name>.<namespace>.svc.cluster.local`
   - Headless Service: One record per pod, each resolving to the pod's IP

2. **Pods**:
   - With default settings: `<pod-ip-with-dashes>.<namespace>.pod.cluster.local`
   - With hostname and subdomain: `<hostname>.<subdomain>.<namespace>.svc.cluster.local`

#### DNS Policy:

Control DNS resolution in pods with `dnsPolicy`:
- `ClusterFirst`: Default - resolve via cluster DNS first, fallback to upstream
- `Default`: Use the node's DNS resolution
- `ClusterFirstWithHostNet`: For pods with hostNetwork=true
- `None`: Use DNS settings specified in `dnsConfig`

#### Custom DNS Configuration:

```yaml
spec:
  dnsPolicy: "None"
  dnsConfig:
    nameservers:
      - 8.8.8.8
    searches:
      - example.com
    options:
      - name: ndots
        value: "5"
```

## Storage

### Volumes

Kubernetes volumes provide a way to store data that outlives a pod's lifetime. When a container crashes, kubelet restarts it, but files are lost. Volumes solve this problem.

#### Core Concepts:

1. **Volume Lifetime**: Tied to pod lifetime, not containers
2. **Data Sharing**: Volumes can be shared among containers in a pod
3. **Independence from Containers**: Data persists through container restarts

#### Volume Types:

1. **emptyDir**:
   - Empty directory created when pod is assigned to a node
   - Data exists as long as the pod runs on that node
   - Useful for scratch space or sharing files between containers
   ```yaml
   volumes:
   - name: cache-volume
     emptyDir: {}
   ```

2. **hostPath**:
   - Mounts a file or directory from the host node's filesystem
   - Persistent across pod restarts on same node
   - Not suitable for most production uses (security concerns)
   ```yaml
   volumes:
   - name: log-volume
     hostPath:
       path: /var/log
       type: Directory
   ```

3. **configMap/secret**:
   - Provide pods with configuration data or sensitive information
   - Changes to ConfigMap/Secret can be propagated to the volume
   ```yaml
   volumes:
   - name: config-volume
     configMap:
       name: my-config
   ```

4. **Cloud-provider-specific volumes**:
   - awsElasticBlockStore, azureDisk, gcePersistentDisk, etc.
   - Integrate with cloud provider storage services
   ```yaml
   volumes:
   - name: gce-volume
     gcePersistentDisk:
       pdName: my-disk
       fsType: ext4
   ```

5. **persistentVolumeClaim**:
   - Request for storage that can be fulfilled by a PersistentVolume
   - Most common and recommended approach
   ```yaml
   volumes:
   - name: data-volume
     persistentVolumeClaim:
       claimName: my-pvc
   ```

6. **Projected Volumes**:
   - Map several existing volume sources into the same directory
   - Combine configMap, secret, downwardAPI, serviceAccountToken
   ```yaml
   volumes:
   - name: projected-volume
     projected:
       sources:
       - secret:
           name: mysecret
       - configMap:
           name: myconfigmap
   ```

#### Using Volumes in Pods:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
spec:
  containers:
  - name: test-container
    image: nginx
    volumeMounts:
    - mountPath: /test-volume     # Where to mount in container
      name: test-volume           # Must match volume name
  volumes:
  - name: test-volume             # Volume name
    emptyDir: {}                  # Volume type and config
```

### PersistentVolumes and PersistentVolumeClaims

PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs) provide an abstraction layer between storage consumption and implementation.

#### Persistent Volume (PV):

- Created by cluster administrators or dynamically provisioned
- Resource in the cluster just like a node
- Has a lifecycle independent of any pod
- Backed by various storage systems (NFS, cloud-provider-specific, etc.)

#### PV Example:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0003
spec:
  capacity:
    storage: 5Gi
  volumeMode: Filesystem       # Filesystem or Block
  accessModes:
    - ReadWriteOnce            # Access mode
  persistentVolumeReclaimPolicy: Recycle  # What to do when claim is released
  storageClassName: slow       # StorageClass name
  mountOptions:                # Mount options for the volume
    - hard
    - nfsvers=4.1
  nfs:                         # Volume type specific parameters
    path: /tmp
    server: 172.17.0.2
```

#### Persistent Volume Claim (PVC):

- Created by users to claim storage resources
- Consumed by pods
- Can specify size, access modes, storage class
- Binds to a PV that matches its requirements

#### PVC Example:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 8Gi
  storageClassName: slow       # Must match PV's storageClassName
  selector:                    # Optional - filter which PVs can be bound
    matchLabels:
      release: "stable"
```

#### Access Modes:

1. **ReadWriteOnce (RWO)**: Volume can be mounted as read-write by a single node
2. **ReadOnlyMany (ROX)**: Volume can be mounted read-only by many nodes
3. **ReadWriteMany (RWX)**: Volume can be mounted as read-write by many nodes

#### Reclaim Policies:

1. **Retain**: Manual reclamation when claim is deleted
2. **Delete**: Delete the volume when claim is deleted
3. **Recycle**: (Deprecated) Basic scrub (rm -rf) before reuse

#### Lifecycle:

1. **Provisioning**: Static (admin pre-creates) or Dynamic (created on-demand)
2. **Binding**: PVC is bound to a matching PV
3. **Using**: Pod uses the PVC as a volume
4. **Releasing**: Pod is deleted, PVC is still available
5. **Reclaiming**: PVC is deleted, PV is reclaimed according to policy

#### Using PVC in Pods:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```

### StorageClasses

StorageClasses provide a way to describe the "classes" of storage offered. Different classes might map to quality-of-service levels, backup policies, or arbitrary policies determined by administrators.

#### Key Features:

1. **Dynamic Provisioning**: Automatically create PVs when PVCs request them
2. **Storage Profiles**: Different storage configurations for different use cases
3. **No Admin Intervention**: Users can create and use storage without admin involvement

#### StorageClass Example:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs  # Plugin that understands how to create the volume
parameters:                         # Parameters passed to the provisioner
  type: gp2
  fsType: ext4
  encrypted: "true"
reclaimPolicy: Delete               # What to do when PVC is deleted
allowVolumeExpansion: true          # Allow volumes to be expanded after creation
mountOptions:                       # Mount options for the volume
  - debug
volumeBindingMode: Immediate        # When to bind and provision a volume
```

#### Common Storage Provisioners:

1. **Cloud-provider-specific**:
   - AWS: `kubernetes.io/aws-ebs`
   - GCP: `kubernetes.io/gce-pd`
   - Azure: `kubernetes.io/azure-disk`

2. **On-premise solutions**:
   - NFS: `nfs.csi.k8s.io`
   - Local Volumes: `kubernetes.io/no-provisioner`
   - OpenEBS: `openebs.io/provisioner-iscsi`
   - Ceph RBD: `ceph.com/rbd`

#### Volume Binding Modes:

1. **Immediate**: Volume binding and dynamic provisioning occurs immediately
2. **WaitForFirstConsumer**: Delay binding and provisioning until a pod using the PVC is created

#### Default StorageClass:

- Marked with annotation: `storageclass.kubernetes.io/is-default-class: "true"`
- Used when PVC does not specify a storage class
- Only one default storage class should exist

#### Using StorageClass in PVCs:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 8Gi
  storageClassName: standard  # Reference to the StorageClass
```

## Configuration and Secrets

### ConfigMaps

ConfigMaps allow you to decouple configuration from image content, making applications more portable.

#### Use Cases:

1. **Environment variables** for pods
2. **Command-line arguments** for containers
3. **Configuration files** in a volume
4. **Source for a Kubernetes resource property**

#### Creating ConfigMaps:

1. **From literal values**:
   ```
   kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
   ```

2. **From a file**:
   ```
   kubectl create configmap my-config --from-file=config.properties
   ```

3. **From multiple files in a directory**:
   ```
   kubectl create configmap my-config --from-file=config-dir/
   ```

4. **Using YAML**:
   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: game-config
   data:
     game.properties: |
       enemies=aliens
       lives=3
     ui.properties: |
       color.good=purple
       color.bad=yellow
   ```

#### Using ConfigMaps:

1. **As environment variables**:
   ```yaml
   spec:
     containers:
     - name: test-container
       env:
       - name: SPECIAL_KEY
         valueFrom:
           configMapKeyRef:
             name: my-config
             key: special.key
   ```

2. **As environment variable collections**:
   ```yaml
   spec:
     containers:
     - name: test-container
       envFrom:
       - configMapRef:
           name: my-config
   ```

3. **As volumes**:
   ```yaml
   spec:
     containers:
     - name: test-container
       volumeMounts:
       - name: config-volume
         mountPath: /etc/config
     volumes:
     - name: config-volume
       configMap:
         name: my-config
   ```

#### ConfigMap Update Behavior:

- When used as volumes, ConfigMap updates propagate to volumes (with some delay)
- When used as environment variables, updates do NOT propagate (require pod restart)
- You can use a ConfigMap controller or watch for changes to implement reloads

### Secrets

Secrets provide a way to store sensitive information like passwords, tokens, or keys.

#### Types of Secrets:

1. **Opaque** (generic): arbitrary user-defined data
2. **kubernetes.io/service-account-token**: service account token
3. **kubernetes.io/dockercfg**: serialized ~/.dockercfg file
4. **kubernetes.io/dockerconfigjson**: serialized ~/.docker/config.json file
5. **kubernetes.io/basic-auth**: credentials for basic authentication
6. **kubernetes.io/ssh-auth**: credentials for SSH authentication
7. **kubernetes.io/tls**: data for a TLS client or server
8. **bootstrap.kubernetes.io/token**: bootstrap token data

#### Creating Secrets:

1. **From literal values**:
   ```
   kubectl create secret generic my-secret --from-literal=username=user --from-literal=password=pass
   ```

2. **From files**:
   ```
   kubectl create secret generic ssl-cert --from-file=ssl-cert=cert.pem --from-file=ssl-key=key.pem
   ```

3. **Using YAML**:
   ```yaml
   apiVersion: v1
   kind: Secret
   metadata:
     name: my-secret
   type: Opaque
   data:
     username: dXNlcg==  # base64 encoded "user"
     password: cGFzcw==  # base64 encoded "pass"
   ```

#### Using Secrets:

1. **As environment variables**:
   ```yaml
   spec:
     containers:
     - name: test-container
       env:
       - name: USERNAME
         valueFrom:
           secretKeyRef:
             name: my-secret
             key: username
   ```

2. **As environment variable collections**:
   ```yaml
   spec:
     containers:
     - name: test-container
       envFrom:
       - secretRef:
           name: my-secret
   ```

3. **As volumes**:
   ```yaml
   spec:
     containers:
     - name: test-container
       volumeMounts:
       - name: secret-volume
         mountPath: /etc/secret
         readOnly: true
     volumes:
     - name: secret-volume
       secret:
         secretName: my-secret
   ```

#### Secret Security Considerations:

1. **Storage**: By default, secrets are stored unencrypted in etcd
   - Enable encryption-at-rest for etcd
   - Use RBAC to limit access to secrets

2. **Transmission**: Secrets are sent to nodes that run pods using them
   - Use network encryption (TLS) for the cluster

3. **Pod Security**: Secrets are exposed to pods
   - Use PodSecurityPolicies to restrict volume types
   - Control which service accounts can use certain secrets

4. **Use External Secrets Managers**:
   - HashiCorp Vault
   - AWS Secrets Manager
   - Azure Key Vault
   - Google Secret Manager

## Resource Management

### Resource Requests and Limits

Resource requests and limits help Kubernetes make scheduling decisions and enforce resource constraints.

#### Resource Types:

1. **CPU**:
   - Measured in CPU units (1 CPU = 1 vCPU/Core)
   - Can specify m (millicpu) - e.g., 100m = 0.1 CPU
   - Compressible resource - containers can be throttled

2. **Memory**:
   - Measured in bytes
   - Can use suffixes: Ki, Mi, Gi, Ti, Pi, Ei
   - Non-compressible resource - containers can be evicted

3. **Ephemeral Storage**:
   - Local storage for temporary files
   - Measured in bytes with same suffixes as memory

4. **Extended Resources**:
   - Custom resources like GPUs or FPGAs
   - Provided by device plugins

#### Resource Requests:

- The amount of resources a container needs
- Used for scheduling decisions
- Guaranteed minimum amount of resources
- Pod will not be scheduled if requests cannot be satisfied

#### Resource Limits:

- Maximum amount of resources a container can use
- Enforced by the kubelet and container runtime
- If a container exceeds its memory limit, it may be terminated
- If a container exceeds its CPU limit, it will be throttled

#### Example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-demo
spec:
  containers:
  - name: resource-demo-container
    image: nginx
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

#### Quality of Service (QoS) Classes:

1. **Guaranteed**:
   - Every container has memory and CPU limits equal to requests
   - Highest priority, least likely to be evicted

2. **Burstable**:
   - At least one container has resource requests, but not all have limits equal to requests
   - Medium priority for eviction

3. **BestEffort**:
   - No container has resource requests or limits
   - First to be evicted under resource pressure

### Resource Quotas and Limits

#### ResourceQuotas:

ResourceQuotas limit the total resources that can be consumed in a namespace.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: my-namespace
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "8"
    limits.memory: 16Gi
```

#### Types of Quotas:

1. **Compute Quotas**:
   - `requests.cpu`, `limits.cpu`
   - `requests.memory`, `limits.memory`
   - `pods`

2. **Storage Quotas**:
   - `requests.storage`: Total storage requested
   - `persistentvolumeclaims`: Total PVCs
   - Storage requests by StorageClass

3. **Object Count Quotas**:
   - Limit number of objects by type: `pods`, `services`, `configmaps`, etc.

#### LimitRanges:

LimitRanges enforce default and min/max resource constraints per pod or container in a namespace.

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
  namespace: my-namespace
spec:
  limits:
  - default:         # Default limits if not specified
      cpu: 500m
    defaultRequest:  # Default requests if not specified
      cpu: 200m
    max:             # Maximum allowed
      cpu: 1
    min:             # Minimum allowed
      cpu: 100m
    type: Container
```

#### LimitRange Types:

1. **Container Limits**: Apply to individual containers
2. **Pod Limits**: Apply to the sum of all containers in a pod
3. **PVC Limits**: Apply to PersistentVolumeClaims

## Security

### Authentication

Kubernetes uses several authentication strategies to identify users.

#### User Types:

1. **Service Accounts**: In-cluster identities managed by Kubernetes
2. **Normal Users**: External identities managed outside Kubernetes

#### Authentication Methods:

1. **Client Certificates**:
   - Enable by passing `--client-ca-file=FILENAME` to API server
   - Certificates must be signed by the cluster's CA
   - Subject CN is used as username, O as group

2. **Bearer Tokens**:
   - JWT tokens in Authorization header
   - OpenID Connect providers
   - Service account tokens

3. **Basic Authentication**:
   - Username/password in Authorization header
   - Not recommended for production

4. **Proxy Authentication**:
   - External authenticating proxy
   - Passes user info in request headers

#### Service Accounts:

Service accounts provide an identity for processes running in pods.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
  namespace: default
automountServiceAccountToken: true  # Auto-mount the token in pods
```

Every namespace has a default service account, but you can create custom ones for specific permissions.

### Authorization

Once authenticated, authorization determines what actions a user can perform.

#### Authorization Modes:

1. **RBAC (Role-Based Access Control)**:
   - Permissions based on roles assigned to users
   - Most common and recommended approach

2. **ABAC (Attribute-Based Access Control)**:
   - Permissions based on attributes (of user, resource, etc.)
   - Requires API server restart to change policies

3. **Node Authorization**:
   - Special-purpose authorizer for kubelet
   - Allows access to resources used by nodes

4. **Webhook Authorization**:
   - Delegates authorization to an external service
   - Service determines whether to allow the request

#### RBAC Components:

1. **Roles**: Define permissions within a namespace
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: Role
   metadata:
     namespace: default
     name: pod-reader
   rules:
   - apiGroups: [""]
     resources: ["pods"]
     verbs: ["get", "watch", "list"]
   ```

2. **ClusterRoles**: Like Roles but cluster-wide (not namespaced)
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRole
   metadata:
     name: secret-reader
   rules:
   - apiGroups: [""]
     resources: ["secrets"]
     verbs: ["get", "watch", "list"]
   ```

3. **RoleBindings**: Bind Roles to users, groups, or service accounts in a namespace
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: RoleBinding
   metadata:
     name: read-pods
     namespace: default
   subjects:
   - kind: User
     name: jane
     apiGroup: rbac.authorization.k8s.io
   roleRef:
     kind: Role
     name: pod-reader
     apiGroup: rbac.authorization.k8s.io
   ```

4. **ClusterRoleBindings**: Bind ClusterRoles cluster-wide
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: read-secrets-global
   subjects:
   - kind: Group
     name: manager
     apiGroup: rbac.authorization.k8s.io
   roleRef:
     kind: ClusterRole
     name: secret-reader
     apiGroup: rbac.authorization.k8s.io
   ```

#### RBAC Best Practices:

1. **Least Privilege**: Grant only necessary permissions
2. **Use Groups**: Assign roles to groups rather than individual users
3. **Namespace Separation**: Use namespaces to isolate environments
4. **Aggregated Roles**: Use ClusterRole aggregation for modular permissions
5. **Avoid Wildcards**: Specify resources and verbs explicitly

### Admission Control

Admission controllers intercept requests to the Kubernetes API after authentication and authorization, but before object persistence.

#### Types:

1. **Mutating Admission Controllers**: Can modify objects (runs first)
2. **Validating Admission Controllers**: Can only validate objects (runs after mutating)

#### Built-in Admission Controllers:

- **NamespaceLifecycle**: Prevents creation of objects in non-existent namespaces
- **LimitRanger**: Enforces resource limits
- **ServiceAccount**: Automounts service account tokens
- **PodSecurityPolicy**: Validates pod security settings
- **ResourceQuota**: Enforces resource quotas
- **DefaultStorageClass**: Sets default storage class for PVCs

#### Custom Admission Webhooks:

1. **MutatingWebhookConfiguration**: External service that can modify objects
   ```yaml
   apiVersion: admissionregistration.k8s.io/v1
   kind: MutatingWebhookConfiguration
   metadata:
     name: my-mutating-webhook
   webhooks:
   - name: my-webhook.example.com
     clientConfig:
       url: "https://my-webhook.example.com/mutate"
     rules:
     - apiGroups: [""]
       apiVersions: ["v1"]
       operations: ["CREATE", "UPDATE"]
       resources: ["pods"]
     admissionReviewVersions: ["v1"]
     sideEffects: None
     timeoutSeconds: 5
   ```

2. **ValidatingWebhookConfiguration**: External service that validates objects
   ```yaml
   apiVersion: admissionregistration.k8s.io/v1
   kind: ValidatingWebhookConfiguration
   metadata:
     name: my-validating-webhook
   webhooks:
   - name: my-webhook.example.com
     clientConfig:
       url: "https://my-webhook.example.com/validate"
     rules:
     - apiGroups: [""]
       apiVersions: ["v1"]
       operations: ["CREATE", "UPDATE"]
       resources: ["pods"]
     admissionReviewVersions: ["v1"]
     sideEffects: None
     timeoutSeconds: 5
   ```

### Pod Security

#### Pod Security Context:

Security context defines privilege and access control settings for pods and containers.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 1000        # User ID to run all containers as
    runAsGroup: 3000       # Group ID
    fsGroup: 2000          # File system group ID for volumes
    supplementalGroups: [4000, 5000]  # Additional group IDs
  containers:
  - name: security-context-container
    image: busybox
    command: [ "sh", "-c", "sleep 1h" ]
    securityContext:
      allowPrivilegeEscalation: false  # Prevent privilege escalation
      capabilities:
        add: ["NET_ADMIN"]             # Add Linux capabilities
        drop: ["ALL"]                  # Drop capabilities
      privileged: false                # Don't run in privileged mode
      readOnlyRootFilesystem: true     # Make root filesystem read-only
      runAsNonRoot: true               # Validate container runs as non-root
      seccompProfile:                  # Security Computing Mode
        type: RuntimeDefault           # Use runtime default seccomp profile