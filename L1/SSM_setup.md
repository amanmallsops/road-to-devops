Attach role to ec2  

# 1 approach
-> systemctl status snap.amazon-ssm-agent.amazon-ssm-agent
-> systemctl enable snap.amazon-ssm-agent.amazon-ssm-agent
-> systemctl status  snap.amazon-ssm-agent.amazon-ssm-agent

refres the page you can connect .

# 2 approach
## uninstall existing ssm agent 
-> sudo snap remove amazon-ssm-agent
## install dep package:
-> cd /tmp
-> wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
-> sudo dpkg -i amazon-ssm-agent.deb
-> sudo systemctl status amazon-ssm-agent
-> sudo systemctl enable amazon-ssm-agent
-> 
