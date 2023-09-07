# Python_Application
simple python HTTP server

We have a very simple python HTTP server that runs on a 7000 port.
When you try to access an application on a 7000 port it will return an HTML file with hello world in it.

Problem statement:-
Create deployment configuration files that satisfy the following requirements

1. We would like to deploy this docker image on Kubernetes
2. We should be able to access applications on port 80 from the browser, You can test this locally also.
3. We expect a minimum of 3 pods of this application to be running all time.
        So maximum pod will be 5, the minimum pod will be 3
        Scaling of pod will be based on CPU
        Target CPU utilization is 60%.
4. I use EKS cluster . My EKS cluster name is LOCODEMO
5. To store our images . I use registry which is ECR and name of my ECR is LocoECR
6. My ecr LocoECR is attached to  EKS which is LOCODEMO
7. To create a docker image . We need to create a docker file first Vi Dockerfile
        FROM python:latest
        RUN touch index.html
        RUN echo "Hello world!" > index.html
        EXPOSE 7000
        CMD python -m http.server 7000
8. Below is the command to create docker image 
        docker build  -t <image_name> .
9. I want to tag this docker image
        docker tag <image_name> <dockerhub_name>/<image_name>
10. Login to ECR
        docker login
        <username_dockerhub>
        <Password_dockerhub>
11. docker push
        docker push <dockerhub_name>/<image_name>
12. Now we need to write a Kubernetes manifests files
      So I am going to create a folder loco-manifests
        mkdir   loco-manifests
        cd  loco-manifests
        mkdir locoapp
        cd locoapp
        vi locoapp.yml
13. command to execute the Kubernetes manifests file 
        kubectl create -f locoapp/
14. To check how many pods are running the command is 
        Kubectl get pods
15. To verify the whether the hpa is working or not We can use this command and check it
        kubectl get hpa
16. kubectl run apache-bench -i --tty --rm --image=httpd -- ab -n 500000 -c 1000 http://loco-demo-app-service.default.svc.cluster.local/
    we can see the Targetcpu% and how many replicaset are running