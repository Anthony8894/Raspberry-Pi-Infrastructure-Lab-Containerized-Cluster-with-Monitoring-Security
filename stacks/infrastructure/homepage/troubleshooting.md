"Error response from daemon: failed to set up container networking: driver failed programming external connectivity on endpoint homepage (22f119bbc3b3db1f033b54c827d48b0e18da8844d69ab201b323ed5bb16bc203): Bind for 0.0.0.0:3000 failed: port is already allocated"


Port 3000 was already in use. 

Changed the port to 3001, since 3000 was used by grafarna. 

So now accessing the dashboard: http://<pi-ip-address>:3001