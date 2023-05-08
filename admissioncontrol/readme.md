Every API request goes through 3 steps:
  1. Authentication
  2. Authorization 
  3. Admission Controls
 
 
Restrictions that must be in place for every cluster.

  1. Don't allow anonymous access
  2. Restrict insecure port
  3. Dont' expose API server to outside
  4. Restrict access from nodes to API(Node Restriction)
  5. Prevent unauthorized access using RBAC
  6. Prevent pods from accessing the API
  7. Prevent port behind firewall/allowed ip ranges(cloud) 

Point 7 can be an external and it depends on your cluster infrastructure 


