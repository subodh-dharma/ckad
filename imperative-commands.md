# Imperative Commands 

## Pods

1. To create a **pod** directly in a given namespace

    ```bash
    $ k run nginx --image nginx -n <ns>
    pod/nginx created
    ```

## Deployments

1. To create **deployment** with different options

   a. Create a simple deployment

    ```bash
    $ k create deploy dep1 --image=nginx
    deployment.apps/dep1 created
    $ k get deploy dep1
    NAME   READY   UP-TO-DATE   AVAILABLE   AGE
    dep1   1/1     1            1           51s
    ```

    b. Create a deployment with replicas

    ```bash
    $ k create deploy dep2 --image=nginx --replicas=4
    deployment.apps/dep1 created
    $ k get deploy
    NAME   READY   UP-TO-DATE   AVAILABLE   AGE
    dep2   4/4     4            4           68s
    ```

    c. Create a deployment with port exposed

    ```bash
    $ k create deploy dep3 --image nginx --port 8080
    deployment.apps/dep3 created
    ```
    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k describe deploy dep3
    Name:                   dep3
    Namespace:              default
    CreationTimestamp:      Sat, 29 Aug 2020 15:11:48 -0700
    Labels:                 app=dep3
    Annotations:            deployment.kubernetes.io/revision: 1
    Selector:               app=dep3
    Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
    StrategyType:           RollingUpdate
    MinReadySeconds:        0
    RollingUpdateStrategy:  25% max unavailable, 25% max surge
    Pod Template:
    Labels:  app=dep3
    Containers:
    nginx:
        Image:        nginx
        Port:         8080/TCP
        Host Port:    0/TCP
        Environment:  <none>
        Mounts:       <none>
    Volumes:        <none>

    ```

    </p>
    </details>

    d. Create a deployment with entrypoint command

    ```bash
    $ k create deploy dep4 --image nginx -- sleep 50000
    deployment.apps/dep4 created
    ```
    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k describe deploy dep4
    Name:                   dep4
    Namespace:              default
    CreationTimestamp:      Sat, 29 Aug 2020 15:33:15 -0700
    Labels:                 app=dep4
    Annotations:            deployment.kubernetes.io/revision: 1
    Selector:               app=dep4
    Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
    StrategyType:           RollingUpdate
    MinReadySeconds:        0
    RollingUpdateStrategy:  25% max unavailable, 25% max surge
    Pod Template:
    Labels:  app=dep4
    Containers:
    nginx:
        Image:      nginx
        Port:       <none>
        Host Port:  <none>
        Command:
        sleep
        50000
        Environment:  <none>
        Mounts:       <none>
    Volumes:        <none>

    ```

    </p>
    </details>

    e. imperative commands using all options

    ```bash
    $ k create deploy dep5 --image nginx --port 8080 --replicas=3 -- sleep 50000
    deployment.apps/dep5 created
    ```
    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k describe deploy dep5
    Name:                   dep5
    Namespace:              default
    CreationTimestamp:      Sat, 29 Aug 2020 15:41:10 -0700
    Labels:                 app=dep5
    Annotations:            deployment.kubernetes.io/revision: 1
    Selector:               app=dep5
    Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
    StrategyType:           RollingUpdate
    MinReadySeconds:        0
    RollingUpdateStrategy:  25% max unavailable, 25% max surge
    Pod Template:
    Labels:  app=dep5
    Containers:
    nginx:
        Image:      nginx
        Port:       8080/TCP
        Host Port:  0/TCP
        Command:
        sleep
        50000
        Environment:  <none>
        Mounts:       <none>
    Volumes:        <none>
    Conditions:
    Type           Status  Reason
    ----           ------  ------
    Available      True    MinimumReplicasAvailable
    Progressing    True    NewReplicaSetAvailable
    OldReplicaSets:  <none>
    NewReplicaSet:   dep5-fc4745f75 (3/3 replicas created)
    Events:
    Type    Reason             Age   From                   Message
    ----    ------             ----  ----                   -------
    Normal  ScalingReplicaSet  20s   deployment-controller  Scaled up replica set dep5-fc4745f75 to 3
    ```

    </p>
    </details>

    f. assign custom labels to deployment in a single command

    ```bash
    $ k create deploy dep6 --image nginx -o yaml --dry-run=client | kubectl label --local -f - environment=qa canary=true -o yaml | kubectl create -f -
    deployment.apps/dep6 created
    ```
    <details>
    <summary>Output</summary>
    <p>

    _Note_: this applies labels only at deployment level and not to the pod template spec which are used as selectors

    ```bash
        $ k describe deploy dep6
        Name:                   dep6
        Namespace:              default
        CreationTimestamp:      Sat, 29 Aug 2020 18:03:58 -0700
        Labels:                 app=dep6
                                canary=true
                                environment=qa
        Annotations:            deployment.kubernetes.io/revision: 1
        Selector:               app=dep6
        Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
        StrategyType:           RollingUpdate
        MinReadySeconds:        0
        RollingUpdateStrategy:  25% max unavailable, 25% max surge
        Pod Template:
        Labels:  app=dep6
        Containers:
        nginx:
            Image:        nginx
            Port:         <none>
            Host Port:    <none>
            Environment:  <none>
            Mounts:       <none>
        Volumes:        <none>
    ```


    </p>
    </details>

    f. Create a deployment and edit a deployment immidiately after creating

    ```bash
    $ k create deploy dep8 --image nginx  -o yaml | k edit -f -
    ``` 

    <details>
    <summary>Output</summary>
    <p>

    This opens a editor after creating a deployment of nginx with 1 replica. In the editor you can update the deployment defintion file and save it.
    This will save and update the deployment on the server.
    </p>
    </details>


## ConfigMaps

1. Imperative commands to create **configmap**

    a. using literal in the command line

    ```bash
    $ k create cm cm1 --from-literal=ENV=development
    configmap/cm1 created
    ```

    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k get cm cm1 -o yaml
    apiVersion: v1
    data:
        ENV: development
    kind: ConfigMap
    metadata:
    ...
    ```
    </p>
    </details>

   b. using a file in the command line

   ```bash
    $ cat cm2.txt
    ENV developer1
    SAFEMODE on
    $ k create cm cm2 --from-file=cm2.txt 
    configmap/cm2 created
    ```

    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k get cm cm2 -o yaml
    apiVersion: v1
    data:
    cm2.txt: |
        ENV developer1
        SAFEMODE on
    kind: ConfigMap
    metadata:
    ...
   ```
    </p>
    </details>

   c. using a env file in the command line

   ```bash
    $ cat cm3.env
    ENV=developer1
    SAFEMODE=on
    $ k create cm cm3 --from-env-file=cm3.env 
    configmap/cm3 created
    ```

    <details>
    <summary>Output</summary>
    <p>

    $ k get cm cm3 -o yaml
    apiVersion: v1
    data:
    ENV: developer1
    SAFEMODE: "on"
    kind: ConfigMap
    metadata:
    ...
    ```
    </p>
    </details>

## Secrets

1. Imperative commands to create **secret**

   a. using from-literal in command line

   ```bash
    $ k create secret generic sec1 --from-literal=user=admin --from-literal=password=pass
    secret/sec1 created
    ```

    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k get secret sec1 -o yaml
    apiVersion: v1
    data:
    password: cGFzcw==
    user: YWRtaW4=
    kind: Secret
    metadata:
    ...
    ```
    </p>
    </details>
   
   b. using files content where file name is key

    ```bash
    $ k create secret generic sec2 --from-file=cm3.txt 
    secret/sec2 created
    ```

    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k get secret sec2 -o yaml
    apiVersion: v1
    data:
    cm3.txt: RU5WPWRldmVsb3BlcjEKU0FGRU1PREU9b24K
    kind: Secret
    metadata:
    ...
    ```
    </p>
    </details>


   c. using files content where key is used and value is content of the files

   ```bash
    $ k create secret generic sec3 --from-file=key=cm3.txt 
    secret/sec3 created
   ```

    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k get secret sec3 -o yaml
    apiVersion: v1
    data:
    key: RU5WPWRldmVsb3BlcjEKU0FGRU1PREU9b24K
    kind: Secret
    metadata:
    ...
    ```
    </p>
    </details>

   d. using .env file to map key=value in secret

   ```bash
    $ k create secret generic sec4 --from-env-file=cm3.env
    secret/sec4 created
   ```

    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k get secret sec4 -o yaml
    apiVersion: v1
    data:
    ENV: ZGV2ZWxvcGVyMQ==
    SAFEMODE: b24=
    kind: Secret
    metadata:
    ...
   ```
    </p>
    </details>


## Services

1. Imperative commands for **service**

   a. Create a ClusterIP service

   ```bash
   $ k create service clusterip svc2 --tcp=9090:8080
   service/svc2 created
   ```
    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k get svc svc2 -o yaml
    ...
    spec:
    clusterIP: 10.0.192.189
    ports:
    - name: 9090-8080
        port: 9090
        protocol: TCP
        targetPort: 8080
    selector:
        app: svc2
    sessionAffinity: None
    type: ClusterIP
   ```
    </p>
    </details>

   b. Create a NodePort service

   ```bash
    $ k create svc nodeport svc3 --tcp=9999:8888 --node-port=30888
    service/svc3 created
   ```
    
    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k get svc svc2 -o yaml
    ...
    spec:
    clusterIP: 10.0.140.136
    externalTrafficPolicy: Cluster
    ports:
    - name: 9999-8888
        nodePort: 30888
        port: 9999
        protocol: TCP
        targetPort: 8888
    selector:
        app: svc3
    sessionAffinity: None
    type: NodePort
    ```
    </p>
    </details>

   c. Add custom selectors to the service and create new service in a single command

   ```bash
    $ k create svc clusterip svc4 --tcp=9292:8282 -o yaml --dry-run=client | kubectl set selector --local -f - 'env=dev,app=dep2,test=true' -o yaml | k create -f -
   ```

    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k get svc svc4 -o yaml
    ...
    spec:
    clusterIP: 10.0.222.105
    ports:
    - name: 9292-8282
        port: 9292
        protocol: TCP
        targetPort: 8282
    selector:
        app: dep2
        env: dev
        test: "true"
    sessionAffinity: None
    type: ClusterIP
    ```
    </p>
    </details>

## Job

1. Imperative commands to create Job

    a. create simple job with an image

    ```bash
    $ k create job job1 --image busybox
    job.batch/job1 created
    ``` 
    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k describe job job1
    Name:           job1
    Namespace:      default
    Selector:       controller-uid=5e67e78a-b761-4b11-b59c-09f7f9b73db1
    Labels:         controller-uid=5e67e78a-b761-4b11-b59c-09f7f9b73db1
                    job-name=job1
    Annotations:    <none>
    Parallelism:    1
    Completions:    1
    Start Time:     Sun, 30 Aug 2020 09:29:51 -0700
    Completed At:   Sun, 30 Aug 2020 09:29:54 -0700
    Duration:       3s
    Pods Statuses:  0 Running / 1 Succeeded / 0 Failed
    Pod Template:
    Labels:  controller-uid=5e67e78a-b761-4b11-b59c-09f7f9b73db1
            job-name=job1
    Containers:
    job1:
        Image:        busybox
        Port:         <none>
        Host Port:    <none>
        Environment:  <none>
        Mounts:       <none>
    Volumes:        <none>
    Events:
    Type    Reason            Age   From            Message
    ----    ------            ----  ----            -------
    Normal  SuccessfulCreate  34s   job-controller  Created pod: job1-bwhs7
    Normal  Completed         31s   job-controller  Job completed
    ```

    </p>
    </details>


    b. create simple job with an image and a command

    ```bash
    $ k create job job2 --image busybox -- sleep 500
    job.batch/job2 created
    ``` 
    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k describe job job2
    Name:           job2
    Namespace:      default
    Selector:       controller-uid=cb2a52ec-c489-48f2-b315-97d39139f85b
    Labels:         controller-uid=cb2a52ec-c489-48f2-b315-97d39139f85b
                    job-name=job2
    Annotations:    <none>
    Parallelism:    1
    Completions:    1
    Start Time:     Sun, 30 Aug 2020 09:39:43 -0700
    Pods Statuses:  1 Running / 0 Succeeded / 0 Failed
    Pod Template:
    Labels:  controller-uid=cb2a52ec-c489-48f2-b315-97d39139f85b
            job-name=job2
    Containers:
    job2:
        Image:      busybox
        Port:       <none>
        Host Port:  <none>
        Command:
        sleep
        500
        Environment:  <none>
        Mounts:       <none>
    Volumes:        <none>
    Events:
    Type    Reason            Age   From            Message
    ----    ------            ----  ----            -------
    Normal  SuccessfulCreate  18s   job-controller  Created pod: job2-bkjq5
    ```

    </p>
    </details>


## Service (again!)

1. You can create service objects using another kubectl command i.e. `expose`

   a. Expose a deployment (or RS, Pod, RC) using the `expose` command to create a service object.

   ```bash
   $ k expose deploy/dep2 --port 8080 --target-port 8080 --name dep-port --protocol TCP
   service/dep-port exposed
   ```
    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k describe svc dep-port
    Name:              dep-port
    Namespace:         default
    Labels:            app=dep2
    Annotations:       <none>
    Selector:          app=dep2
    Type:              ClusterIP
    IP:                10.0.244.204
    Port:              <unset>  8080/TCP
    TargetPort:        8080/TCP
    Endpoints:         10.244.0.5:8080,10.244.0.6:8080,10.244.1.7:8080 + 1 more...
    Session Affinity:  None
    Events:            <none>
    ```

    </p>
    </details>

   b. This command is also used to expose an existing service. This command helps to create a new service from an existing service by using the label selectors in the source service. You can expose new ports. Ideal scenario can be to expose one service for `https` and on for `http` without creating a new service from scratch.

   ```bash
    $ k expose service svc2 --name svc5 --port 8899 --target-port 9090 
    service/svc5 exposed
   ```
    <details>
    <summary>Output</summary>
    <p>

    ```bash
    $ k describe svc svc2
    Name:              svc2
    Namespace:         default
    Labels:            app=svc2
    Annotations:       <none>
    Selector:          app=svc2
    Type:              ClusterIP
    IP:                10.0.192.189
    Port:              9090-8080  9090/TCP
    TargetPort:        8080/TCP
    Endpoints:         <none>
    Session Affinity:  None
    Events:            <none>

    $ k describe svc svc5
    Name:              svc5
    Namespace:         default
    Labels:            app=svc2
    Annotations:       <none>
    Selector:          app=svc2
    Type:              ClusterIP
    IP:                10.0.9.180
    Port:              <unset>  8899/TCP
    TargetPort:        9090/TCP
    Endpoints:         <none>
    Session Affinity:  None
    Events:            <none>
    ```

    </p>
    </details>

# Labels

  1. Use the following commands to assign labels to resources. You can assign new labels, reassing values and remove label for these resources.

   a. Assign labels

   ```bash
    $ k label pod nginx app=nginx env=dev
    pod/nginx labeled
   ```

   b. Overwrite / Reassign values to the labels

   ```bash
    $ k label pod nginx --overwrite env=qa
    pod/nginx labeled
   ```

   <details>
   <summary>Output</summary>
   <p>

   ```bash
    $ k get po --selector run=nginx --show-labels
    NAME    READY   STATUS    RESTARTS   AGE   LABELS
    nginx   1/1     Running   0          24h   app=nginx,env=qa,run=nginx
   ```

   </p>
   </details>


   c. Remove existing labels from a resource

   ```bash
   $ k label pod nginx env-
   pod/nginx labeled
   ```

   <details>
   <summary>Output</summary>
   <p>

   ```bash
    $ k get po --selector run=nginx --show-labels
    NAME    READY   STATUS    RESTARTS   AGE   LABELS
    nginx   1/1     Running   0          24h   app=nginx,run=nginx
   ```

   </p>
   </details>


# Environment Variables (A shortcut!)

 1. Use `kubectl set env ` command to set the environment variable to pod templates of the resources like _deploy, rs, rc, job_

    a. Set new env on a pod

    ```bash
    $ k set env deploy dep1 LOG_DIR=/var/log
    deployment.apps/dep1 env updated
    ```

    <details>
    <summary>Output</summary>
    <p>

    ```bash
        $ k describe deploy dep1
        Name:                   dep1
        Namespace:              default
        CreationTimestamp:      Sat, 29 Aug 2020 15:07:55 -0700
        Labels:                 app=dep1
        Annotations:            deployment.kubernetes.io/revision: 2
        Selector:               app=dep1
        Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
        StrategyType:           RollingUpdate
        MinReadySeconds:        0
        RollingUpdateStrategy:  25% max unavailable, 25% max surge
        Pod Template:
        Labels:  app=dep1
        Containers:
        nginx:
            Image:      nginx
            Port:       <none>
            Host Port:  <none>
            Environment:
            LOG_DIR:  /var/log
            Mounts:     <none>
        Volumes:      <none>
    ```

    </p>
    </details>

    By repeated applying new env variables the list of ENV can be extended.

    b. To replace an existing env variable value

    ```bash
    $ k set env deploy dep1 --overwrite ENV=qa
    deployment.apps/dep1 env updated
    ```

    This will reassign the value for existing ENV variable `ENV`. If it doesn't exists it will create a new environment variable.

    c. To delete any existing env variable

    ```bash
    $ k set env deploy dep1 ENV-
    deployment.apps/dep1 env updated
    ```

    d. To update any specific container in a multi-container pod, use `--containers` flag

    ```bash
    $ k set env deploy dep1 --containers nginx-backup BACKUP=false
    deployment.apps/dep1 env updated
    ```

# Node Taints

  1. There are some questions which ask to schedule a pod/deployment on a specific node. In that case the node taints are useful.
  2. Node Taints along with Pod Tolerations are used to schedule pods on a specific node.

     a. To add a taint to any node

     ```bash
     $ k taint node aks-agentpool-27254894-vmss00000 nginxonly=yes:NoSchedule
     node/aks-agentpool-27254894-vmss000000 tainted
     ```

     <details>
     <summary>Output</summary>
     <p>

     ```bash
        $ k describe node aks-agentpool-27254894-vmss000000
        Name:               aks-agentpool-27254894-vmss000000
        Roles:              agent
        Labels:             agentpool=agentpool
        Annotations:        node.alpha.kubernetes.io/ttl: 0
                            volumes.kubernetes.io/controller-managed-attach-detach: true
        CreationTimestamp:  Sat, 29 Aug 2020 13:57:16 -0700
        Taints:             nginxonly=yes:NoSchedule
        Unschedulable:      false
        Lease:

     ```

     </p>
     </details>

     A pod can be scheduled, but not guaranteed, on this node if it has appropriate tolerations like following:

     ```yaml
        apiVersion: v1
        kind: Pod
        metadata:
        labels:
            app: nginx
            run: nginx
        name: nginx-tainted
        namespace: default
        spec:
        containers:
            - image: nginx
            imagePullPolicy: Always
            name: nginx
        restartPolicy: Always
        tolerations:
            - effect: "NoSchedule"
            key: "nginxonly"
            operator: "Exists"
     ```
