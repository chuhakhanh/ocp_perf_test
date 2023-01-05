# ocp_perf_test

# references
    
    https://github.com/IBM/k8s-storage-perf

# setup

## docker on centos 7



    https://phoenixnap.com/kb/how-to-install-docker-centos-7

    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum install docker -y
    systemctl start docker
    systemctl enable docker
    systemctl status docker

### create docker image

    docker build -t ansible-test:1 --force-rm -f ansible.dockerfile .


### push to docker hub

Login to https://hub.docker.com/repository/docker/chuhakhanh/
Create repositoy ansible


    # docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    ansible-test        1                   ac82cbeefab2        2 minutes ago       1.1 GB
    docker.io/centos    8                   5d0da3dc9764        15 months ago       231 MB
    
    # docker tag ansible-test:1 chuhakhanh/ansible:2.10
    # docker images
    REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
    ansible-test         1                   ac82cbeefab2        6 minutes ago       1.1 GB
    chuhakhanh/ansible   2.10                ac82cbeefab2        6 minutes ago       1.1 GB
    docker.io/centos     8                   5d0da3dc9764        15 months ago       231 MB
    
    # docker push chuhakhanh/ansible:2.10

### Copy openshift client

    docker run -d --name deploy-1 ansible-test:1
    docker exec -it deploy-1 /bin/bash
    ansible-galaxy collection install operator_sdk.util
    ansible-galaxy collection install community.kubernetes
    
    vi ~/.bashrc 
    alias ll='ls -lG'

    scp 10.144.101.103:/usr/local/bin/oc /usr/local/bin/oc
    scp 10.144.101.103:/etc/hosts /etc/hosts
    scp -r 10.144.101.103:/root/ocp-installation /root/ocp-installation

    oc login -u kubeadmin -p RuZx9-4woQB-AsXeQ-k8bF9 https://api-int.cp4d.datalake.vnpt.vn:6443

    oc label node worker21.cp4d.datalake.vnpt.vn "k8s-storage-perf-node=test-node" --overwrite
    oc label node worker22.cp4d.datalake.vnpt.vn "k8s-storage-perf-node=test-node" --overwrite
    oc label node worker24.cp4d.datalake.vnpt.vn "k8s-storage-perf-node=test-node" --overwrite
    oc label node worker25.cp4d.datalake.vnpt.vn "k8s-storage-perf-node=test-node" --overwrite