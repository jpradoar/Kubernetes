# HPA

### Run
	kubectl apply -f HPA/main.yaml


<pre>
NAME                            READY   STATUS    RESTARTS   AGE
pod/demo-hpa-564d945c78-8rwrz   1/1     Running   0          115s

NAME                     TYPE           CLUSTER-IP      EXTERNAL-IP                                                                     PORT(S)        AGE
service/myapp-demo-hpa   LoadBalancer   17.2.8.1    xxxxxxxxxxxxxxxxxxx.elb.us-east-99.amazonaws.com   80:32566/TCP   112s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/demo-hpa   1/1     1            1           117s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/demo-hpa-564d945c78   1         1         1       117s

NAME                                           REFERENCE             TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/demo-hpa   Deployment/demo-hpa   3%/50%    1         10        1          116s
</pre>



# Test
	 while sleep 0.01; do wget -q -O- http://xxxxxxxxxxxxxxxxxxx.elb.us-east-99.amazonaws.com; done
