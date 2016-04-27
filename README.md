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

Step 3: Launch Application (or import template and use GUI)

$ oc new-app -f https://raw.githubusercontent.com/andyneeb/eap-clustering/master/openshift/eap-clustering.yaml

Step 4: Once Deployed, browse the application URL

The web application displays the following information:
- Session ID
- Session counter and timestamp (these are variables stored in the session that are replicated)
- The container name that the web page and session is being hosted from

Step 5: Select the Increment Counter link

Step 6: Kill the running container

$ oc delete pod eap-clustering-n-xyz

Step 7: (Optional) Follow EAP log to see the new pod joining the cluster

$ oc logs eap-clustering-m-abc -f

Step 8: HitRefresh link in the web page

You will see (after a short pause) the same web session being served up from a new container
