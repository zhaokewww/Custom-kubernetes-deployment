# Custom-kubernetes-deployment

====Before use these script=====
1) Make sure you can compile/build/deploy k8s code successfully ONCE on your master
   Follow below link:
   https://openalm.lmera.ericsson.se/plugins/mediawiki/wiki/distrcontplatform/index.php/Custom_K8s_compile,_deploy_and_debug#Federation_Deployment

2) Use your build server (or VM) as your k8s master, and k8s federation control plane host

3) Use another server (or VM) as your node (minion)

4) Put related scripts to your $HOME, and make it executeable. refer below for details

5) These scripts are wriiten in shell/yaml, and verifiefd on k8s 1.8.x


====How to use these scripts=====
1) make-k8s
   Purpose:
	build custom k8s, including basic one and federation one
   Usage:
	make-k8s <build-type> <build-version> <cloud-provider>
	P.S. only useful on master
   Example:
      	make-k8s basic v1.8.0 test	//build basic one
      	make-k8s federation v1.8.0 test	//build federation


2) master-deploy
   Purpose:
	deploy custom k8s, on master
   Usage:
	master-deploy <your-local-passwd> <build-version>
	P.S. only useful on master
   Example:
      	master-deploy ekezhao v1.8.0


3) node-deploy
   Purpose:
	deploy custom k8s, on minion
   Usage:
	make-k8s <your-local-passwd> <build-version>
	P.S. only useful on minion
   Example:
      	node-deploy ekezhao v1.8.0 155.53.28.131 ekezhao ekezhao

4) master-deploy-from-node
   Purpose:
	deploy custom k8s, on a minion node other than k8s build server
   Usage:
	master-deploy-from-node <your-local-passwd> <build-version> <master-ip> <master-user> <master-passwd> <need-copy-build-from-master-or-not>
	P.S. only useful on minion
   Example:
      	master-deploy-from-node ekezhao v1.8.0 155.53.28.131 ekezhao ekezhao YES



5) etcd.yaml
   Purpose:
	create etcd which is used by custom k8s federation control plane
   Usage:
	kubectl create -f etcd.yaml
        P.S. execute kubectl on master, before execute the script: fed-deploy
        P.S. only useful on master
   Example:
      	fed-join ekezhao 155.53.28.131 ekezhao ekezhao YES


6) coredns.yaml
   Purpose:
	create coredns which is used by custom k8s federation control plane
   Usage:
	kubectl create -f coredns.yaml
        P.S. execute kubectl on master, before execute the script: fed-deploy
        P.S. only useful on master
   Example:
      	fed-join ekezhao 155.53.28.131 ekezhao ekezhao YES


7) join-fed.tempalte
   Purpose:
	create scripts which is used by other clusters to join federation
   Usage:
	No need to execute, it will be usesd by script: fed-deploy automatically
        P.S. only useful on master


8) fed-deploy
   Purpose:
	deploy custom k8s federation control plane
   Usage:
	fed-deploy <federation-name> <zone-domian-name> <your-local-passwd> <build-version>
	P.S. only useful on master
   Example:
      	fed-deploy fellowship ericsson.com. ekezhao v1.8.0

9) fed-join
   Purpose:
	join or unjoin from custom k8s federation control plane
   Usage:
	fed-join <your-local-passwd> <master-ip> <master-user> <master-passwd> <join-or-unjoin>
	P.S. only useful on remote cluster
   Example:
      	fed-join ekezhao 155.53.28.131 ekezhao ekezhao YES
   
