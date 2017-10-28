Week 3 Homework: Data Models

Again, trying to stay simple and building on it later on (if time permits)

jwilkes@ubuntu:~/NETWORKAUTOMATIONSOLUTIONS/DataModels (Wk3HW)$ tree
.
├── ansible.cfg
├── baseline.j2
├── build_dir
│   ├── R1
│   │   └── main.conf
│   ├── R2
│   │   └── main.conf
│   └── R3
│       └── main.conf
├── build.yml
├── config_dir
├── fabric.yml
├── group_vars
│   └── all.yml
├── hosts
├── host_vars
│   ├── nodes.yml
│   ├── R1.yml
│   ├── R2.yml
│   └── R3.yml
├── services.yml
├── templates
│   ├── 0baseline.j2
│   └── maintemp.j2
└── template.yml

Originally thought of the following for Data Models:
Your nodes  - Device name, IPs - individual yml files under /host_vars/
Common network parameters  - credentials, domain, gw, snmp, vty login local - /group_vars/all.yml
Infrastructure - Can setup routing (EIGRP) - fabric.yml
Service you want to model - QoS on one or two device - services.yml

However, after trying to keep this simple, I found that there was really only a base configuration and determination if the device 
needed QoS configuration or not (determined by hosts groups).  I would like to build on this but would have to think of scenarios
in which different devices would need different infrastructure and service configurations.

Also, I was trying to figure out in this process (will have to think for the next exercise), if I want to keep with multiple data models
(like the DMVPN example) or just one (like the OSPF examples).  Either way, I was able to create the configuration using the 
build.yml playbook and the maintemp.j2 Jinja2 template.  I have tested this and it works.
