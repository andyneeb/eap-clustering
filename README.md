Create a clustered JBoss EAP application with session replication
===================

Session Replication
===================
OpenShift supports scalable applications. Scaling enables your application to react to changes in load and automatically allocate the necessary resources to handle the current demand. The OpenShift Enterprise infrastructure monitors load and can automatically add an additional container to handle requests. 

For scaled applications, the JBoss App Server leverages the clustering support already available in the JBoss App Server. As a result, when your application uses web session data, the JBoss App Server container will automatically replicate the data. As a developer, you simply configure your web application to use the distributable tag.

This repo contains sample files for testing session replication.

Demo Instructions
===================
Step 1 - Create Project

$ oc new-project eap-clustering --display-name="EAP Web Session Replication Demo" --description="EAP Web Session Replication Demo"

Step 2: Provide EAP Access to Kubernetes API

$ oc policy add-role-to-user view system:serviceaccount:$(oc project -q):default -n $(oc project -q)

Step 3: Upload Template

$ oc create -f https://raw.githubusercontent.com/andyneeb/eap-clustering/master/openshift/eap-clustering.yaml

Step 4: Launch Application (or use GUI)

$ oc new-app eap-clustering

Step 5: Browse to the application itself, a route should have been created

The web application displays the following information:
- Session ID
- Session counter and timestamp (these are variables stored in the session that are replicated)
- The container name that the web page and session is being hosted from

Step 6: Select the Increment Counter link

Step 7: Kill the running container

$ oc delete pod eap-clustering-<n-xyz>

Step 8: Wait for replication controller to bring up another instance

Optional: Follow EAP (container) log to see the new pod joining the cluster 

Step 9: HitRefresh link in the web page

You will see (after a short pause) the same web session being served up from a new container
