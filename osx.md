#os x cli

#Get a list of network services
networksetup listallnetworkservices

#Set DNS
networksetup -setdnsservers "${1}" 8.8.8.8 8.8.8.4
