# Springboot CRUD application with DOCKER & K8s

![image](https://user-images.githubusercontent.com/10458982/188603047-eb886bde-d71a-4555-8c90-97487564726e.png)

![image](https://user-images.githubusercontent.com/10458982/188603856-bbd0e471-f541-4cad-aa62-717832636624.png)

![image](https://user-images.githubusercontent.com/10458982/188604616-351892a6-d459-48bc-b9db-a8320c3d9401.png)

![image](https://user-images.githubusercontent.com/10458982/188604887-c69af3e9-5e0a-4e7e-ac91-4476d92896b7.png)

![image](https://user-images.githubusercontent.com/10458982/188605268-c519eb03-146d-4051-a31e-ed978c3fbf44.png)

----------------------------------------------------
Notes necessary commands on docker & kubernetes:
----------------------------------------------------

Step 1 : SpringBoot CRUD App need to connect for MySQL Instance which is running in kubernetes pods:

	      -> Install MySQL Instance to kubernetes pods
	      
              -> Start MiniKube -> CMD : minikube start (this command will start kubernetes in the machine)
	      
              -> Sync docker and minikube -> CMD : eval $(minikube docker-env)
	      
              -> check docker images -> CMD : docker images (will list out all the docker images)
	      
              -> Pull MySQL image from dockerhub (done by using the deployment object by specifying the lastest MySQL version, so that kubernetes deployment object                       will take care pull process and execute in seperate pod)
	      
	      -> create deployment & service object using yaml file (refer : db-deployment.yaml file from code base)
	      
	      -> to run MySQL instanse in kubernetes pods we need to create deployment object and to expose that instanse outside kubernetes we need to create service                    object
	      -> Navigate to project directory and execute db-deployment.yaml file 
	      
	      -> CMD : kubectl apply -f db-deployment.yaml (this will create persistanceVolume claim, deployment object & Service Object)
	      
							-> Verify deployment = CMD : kubectl get deployments
							
							-> Verify pods = CMD : kubectl get pods
							
							-> Check logs = CMD : kubectl logs <instance NAME> (can verify whether MySQL instance started/not via logs)
							
				-> login to MySQL instance and verify by using command : kubectl exec -it <POD-NAME> /bin/bash
				
				-> login to MySQL using command : mysql -h <HOSTNAME> mysql -u <USERNAME> -p (enter and provice PASSWORD)
				
				-> show databases
				
				-> use <DB_NAME>
				
        ---MySQL Instance will run---
        
Step 2 : Deploy Spring Boot App to kubernetes pods:

          -> Create docker image, deployment object & Service object of SpringBootApp
	  
		      -> create docker image
		      
		      -> build a docker image by using command : docker build -t <dockerImage-file-name>:1.0 .
		      
			  -> check images by using command : docker images (will display all the image list)
			  
			  -> create the deployment & service object using yaml file (refer : app-deployment.yaml from code base)
			  
			  -> execute app-deployment.yaml file using command : kubectl apply -f app-deployment.yaml (this will create deployment & service object)
			  
			  -> verify deployment, service(svc) & pods using command 
			  
					-> kubectl get deployments
					
					-> kubectl get svc
					
					-> kubectl get pods (replicas will be created in pods)
					
			  -> check logs in pods using command : kubectl logs <pod-name>
			  
		-> Check the Node IP & Port to access the application
					-> kubectl get svc (will give the port where SpringBootApp is running)
					
					-> minikube ip (will give the IP of the node)
					
		-> Verify MySQL
				-> Login : mysql -h <HOSTNAME> mysql -u <USERNAME> -p
				
				-> show databases;
				
				-> use <DB-NAME>;
				
				-> execute the select statement : select * <table_name>
				
		-> Sample URL : http://NodeIP:NodePort/functionName
		
		-> Checking the health of each component:
		
				-> execute the command : minikube dashboard (this will re-direct to kubernetes dashboard)
				
----------------------------------------------------
Use of ConfigMap & Secrets in K8s
----------------------------------------------------

![image](https://user-images.githubusercontent.com/10458982/200140238-a43c42cb-9581-4cf3-b497-738e0cbbcf8f.png)



