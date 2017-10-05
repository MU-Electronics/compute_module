  # Setup static ip
  
  Add a static IP, by example at The University of Manchester
  
  Edit the the below file
  ```bash
  sudo nano /etc/dhcpcd.conf
  ```
  
  Add the below
  ```
  interface eth0
  static ip_address=130.88.237.42/16
  static routers=130.88.237.250
  static domain_name_servers=130.88.13.8
  static broadcast=130.88.237.250
  ```
  
  Reboot Pi
  ```bash
  sudo reboot -n
  ```
  
