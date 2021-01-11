# Rapid Fire

This is a list of questions which you can time it yourself for practising the implementation on your local clusters.

This will help speed up the kubectl skills to create the k8s resources which comes handy during the examination.

1. Create a new pod with the NGINX image
2. Create a new pod with the name 'redis' and with the image 'redis:1.99'
3. Identify the problem with the pod
4. Rectify the problem with the pod and wait until pod is ready and healthy
5. Create a new replicaset rs-1 using image nginx with labels tier:frontend and selectors as tier:frontend. Create 3 replicas for the same.
6. Scale the replica set above to 5 replicas.
7. Update the image of the above replicaset to use nginx:alpine image.
8. Scale down the replica set to 2 replicas
9. Create a new deployment deploy-1 using image busybox with 3 replicas and command "sleep 3600"
10. Update the image of above deployment with image ubuntu.
11. Create a namespace 'finance'
12. Create a pod redis using image redis in 'finance' namespace
13. Deploy a redis pod using the redis:alpine image with the labels set to tier=db
14. Create a service redis-service to expose the redis application within the cluster on port 6379.
15. Create a new pod called custom-nginx using the nginx image and expose it on container port 8080
16. Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80.
17. Create a pod with the ubuntu image to run a container to sleep for 5000 seconds
18. Create a new ConfigMap for the 'webapp-color' POD. Set key value pairs as COLOR=blue
19. Create a deployment using nginx which has environment variable CREATED_BY with value octocat
20. Verify the environment variable in the containers of above deployment
21. Create a new config map db-config with key value pairs as a] db-host = "localhost.example.com" b] db-port = 3306 c] db-name = "kubernetes-objects"
22. Mount the config map above in a deployment with image ubuntu which has 2 replicas.
23. Create a deployment which has two containers, one container used nginx image and has port 80 exposed. Mount the config map in (21) as environment variables. Create the second container in this deployment with image ubuntu, keep the container alive using the command `sleep 5000` and mount the config map in (21) in the container with different keys where the new keys are db-host --> DB_URL, db-port --> DB_PORT, db-name --> DB_NAME
24. Create a secret with following details: db_user=root,db_password=pass404, db_role=admin
25. Mount this secret in the deployment created in (23) as environment variable in the ubuntu container only with same keys as mentioned in the secrets.
26. Create a pod with image ubuntu and run the command `sleep 3400` as user 1001
27. Create a pod with image ubuntu and add capabilities for SYS_TIME to each container. Run command `date -s '01JAN 2020 10:00:00' to verify if it is successfully capable.
28. Taint one of the worker nodes in your cluster with the following property, mode=maintainance:NoSchedule. Create a pod in the cluster with image nginx:alpine and ensure the pod runs on the tainted node as above.
Hint: You might want to use tolerations and nodeselectors both in case you have multiple worker nodes.
29. Create a pod with image busybox and command "sleep 30; touch /tmp/debug; sleep 3600". The pod should not be ready to serve until the file /tmp/debug is available in the pod. If the file doesn't exist the pod must fail.
30. Create a pod with image nginx, make sure the server is up and running every 10 seconds. The nginx server ideally takes 30 seconds to warm up so ensure that there are no false negatives for health of the pod.
Note: Nginx runs on port 80.
31. This is a multistage problem: a] Create a deployment with image nginx with port 80 exposed. b] Update the deployment with image nginx:dev and make sure you record the execution of this action. c] If the deployment in step b is unsuccessful, rollback the deployment to previous state with setting any fields explicitly. d] If the nginx deployment is in healthy state, scale the deployment to run 3 replicas, also record this execution for audit-keeping.
32. Create a job which will roll a dice using image kodekloud/throw-dice. The image will generate a random number between 1 and 6 and will exit. It is a success only if it is a 6 else it is a failure. Check how many times it takes the job to trigger to get a successful outcome.
33. Create a job with the above image to generate 8 successful outcomes. Also ensure that the job will attempt no more than 15 attempts and see if the job is successful. You can add an element of 3 parallel executions if required.
34. Create a scheduled job such that it runs every minute. Use the image kodekloud/throw-dice to check the outcome.
