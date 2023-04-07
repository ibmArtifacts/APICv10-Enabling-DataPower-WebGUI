# APICv10-Enabling-DataPower-WebGUI-on-OCP  
Note: These instructions should work on k8s as well via kubectl commands.
#### Asumptions:  
- The Gateway has been deployed successfully on OCP or k8s either as an APIC component or stand-alone gateway.  
  

### Enabled the WebGUI in the gatewaycluster CR    
1. Issue the following command to edit the gateway cluster yaml `oc edit gatewaycluster` then locate the `webGUIManagementEnabled: false` and update the value to *true*. Save the file with `:wq!`.  
  
![image](https://user-images.githubusercontent.com/66093865/230667595-968b923d-95f2-406d-bdc1-6ce5cddbdc03.png)  
  
2. Restart the gateway pod  
![image](https://user-images.githubusercontent.com/66093865/230668144-dde1d175-e53b-4a45-bccc-3a3bca4225f9.png)  
  
### Create the OCP route to access the WebGUI  
3. Create the DataPower WebGUI route by first finding the service (svc) that the DataPower cluster is running on with the cluster-up assigned and with the enabled 9090 port:  
`oc get svc | grep gw`  
![image](https://user-images.githubusercontent.com/66093865/230669148-edbb770d-4733-43b2-914a-87e60541a809.png)  
  
4. Create the route:  
`oc create route passthrough datapower-ui --service=<datapower-svc-name-here> --port=9090`  
![image](https://user-images.githubusercontent.com/66093865/230674116-098c585d-125c-4a20-ac05-5adeca123459.png)  
  
5. Once completed, issue `oc get routes` to get the datapower-ui url to log in.  
![image](https://user-images.githubusercontent.com/66093865/230675259-a11e44ba-226f-489f-9a07-9373be7ac764.png)  
  
### Obtain the Admin password  
6. Search for the name of the secret that is assigned to the admin account of the gateway (normally the secret would be titled with something like `*gw-admin*`).  
  
`oc get secret | grep gw`  
  
![image](https://user-images.githubusercontent.com/66093865/230657088-aaa50ce6-9fbe-4d0b-8ca7-39f2a611f09b.png)  
  
7. Then use the secret name to get the password with the following command:  
  
`oc get secret <gw-admin-secret-name-here> -o jsonpath='{.data.password}' | base64 -d`  
  
![image](https://user-images.githubusercontent.com/66093865/230657050-00fec3c1-9485-482e-964b-aec20e783f58.png)  
  
