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