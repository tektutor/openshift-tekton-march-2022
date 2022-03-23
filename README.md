# CI/CD with TekTon on OpenShift

For training/consulting/coaching, you may reach me

<pre>
    jegan@tektutor.org
    +91 822-000-5626 (WhatsApp)
</pre>

## Interested in checking my Kubernetes articles ( Give me a clap and follow in medium if you found the articles useful )
https://medium.com/tektutor

### My Articles
Setting up a 3 node Kubernetes Cluster on your local machine
```
https://medium.com/tektutor/kubernetes-3-node-cluster-setup-50943378be41
```

Using MetalLB Load Balancer in bare-metal Kubernetes Cluster
<pre>
https://medium.com/tektutor/using-metal-lb-on-a-bare-metal-onprem-kubernetes-setup-6d036af1d20c
</pre>

Using Nginx Ingress Controller in bare-metal Kubernetes Cluster
<pre>
https://medium.com/tektutor/using-nginx-ingress-controller-in-kubernetes-bare-metal-setup-890eb4e7772
</pre>

Deploying Stateful applications in a Kubernetes Cluster
<pre>
https://medium.com/tektutor/deploying-stateful-applications-in-kubernetes-8ffd46920b55
</pre>

Understand the different between Container Engine and Container Runtime
<pre>
https://medium.com/tektutor/container-engine-vs-container-runtime-667a99042f3
</pre>

## Requests and suggestions to the participants

- Please be interactive.  I need your support to deliver this training effectively.  
- Please ask questions. 
- Please feel free to turn on your WebCamera when you can and if possible
- When Trainer asks, do you have any doubts, please respond Yes or No either orally or via Chat
- When Trainer asks, Am I going too fast, should I slow down please respond. Don't hesitate to give your opinion.
- When Trainer asks, Did you complete the lab exercise? Shall we move no to the next topic?  Please respond via chat or speak up
- When Trainer asks, do you have any doubts on the topics covered so far, please be expressive.
- If the lab machine isn't accessible, please inform the trainer.
- At any point of time, please feel to share your screen in case you are struck or would like to demonstrate that you have completed an exercise.

## ⛔ Need your attention - You don't have to install OpenShift in our Training Lab
<pre>
- Our training lab environment already has OpenShift Cluster pre-installed
- Hence, you don't have to perform any installation listed below
- The installation procedures listed below are meant for your future reference
- Any attempt to perform OpenShift installation in our lab environment will corrupt our OpenShift cluster
</pre>

## 🔴🔴 Do and Don'ts, please don't get offended ⛔⛔
<pre>
- Kindly stick to the credential details given to you.  
- Please avoid switching from one user to other user. 
- Please avoid switching between the clusters.
- Please create only one project per participant as the Cluster is shared by 10 participants.
- Please use your name as the project name so we can easily manage them.
- At any point in time, please do not scale beyond 5 pods per participants, imagine 5 x 20 participants = 100 Pods.
  Once you are done with a lab exercise, please delete the project and recreate with the same name.
</pre>

## What will happen if I don't follow the above requests :question::question::question:
<pre>
- When you switch using other user credentials, actually you will end up using other participant's lab machine.  
- It will be confusing for both of you, as the projects gets switched in your lab everytime other participant 
  switches to his/her project and vice versa on other participants lab environment. 
- There is a chance that either you or the other participant may delete each other projects.
- It will overload our cluster leading to many of our deployments crashing. 
- This might even bring down our cluster altogether. 
- We will lose lot of time in fixing the cluster.
</pre>

🧑‍🤝‍🧑 Hence your kind co-operation is requested to deliver this training smoothly.

## ℹ️ OpenShift Installation Options
1. RedHat OpenShift Code Ready Containers (CRC) - Ideal for self-learning purposes only 🎓👨‍🎓👩‍🎓
2. RedHat OpenShift Developer Sandbox - Ideal for self-learning purposes only 🎓👨‍🎓👩‍🎓
3. Installer Provisioned Infrastructure (IPI) - Ideal for R&D 🚀🛰️ , Development 🕵️‍♀️🕵️‍ and Production ⛈️⛅🌕
4. User Provisioned Infrastructure (UPI) - Ideal for Learning 👨‍🎓👨‍🎓👩‍, R&D 🚀🚀, Development 🕵️‍♀️🕵️‍ & Production ⛈️⛅🌕

## ℹ️ Installing RedHat OpenShift Code Ready Containers (CRC)
:x: Please don't attempt this in our training lab as this may corrupt our OpenShift cluster.  The instructions are captured here for your future reference, i.e in case you wish to try this at home post the training.

This will create a virtual machine using KVM and minimal openshift components are installed within the virtual machine. You need to make sure nested VM is enabled before installing CRC.

The cluster will have a single node that supports both master and worker roles.

##### ℹ️ Installing kubectl
:x: Please do not try this in our lab environment as it will corrupt our OpenShift cluster installation.  These instructions are here to help you in setting up OpenShift in your personal laptop/desktop post the training for your self-learning purposes only.

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/bin
```
When prompted for password, type administrator password of your Linux OS.

##### ℹ️ Installing Code Ready Containers in Linux
:x: Please do not try this in our lab environment as it will corrupt our OpenShift cluster installation.  These instructions are here to help you in setting up OpenShift in your personal laptop/desktop post the training for your self-learning purposes only.

```
cd /home/alchemy/Downloads
tar xvf crc-linux-amd64.tar.xz
cd crc-linux-1.38.0-amd64
./crc setup
```

The expected output is
<pre>
jegan@ubuntu:~/Downloads/crc-linux-1.38.0-amd64$ <b>./crc setup</b>
INFO Checking if running as non-root              
INFO Checking if running inside WSL2              
INFO Checking if crc-admin-helper executable is cached 
INFO Checking for obsolete admin-helper executable 
INFO Checking if running on a supported CPU architecture 
INFO Checking minimum RAM requirements            
INFO Checking if crc executable symlink exists    
INFO Checking if Virtualization is enabled        
INFO Checking if KVM is enabled                   
INFO Checking if libvirt is installed             
INFO Checking if user is part of libvirt group    
INFO Checking if active user/process is currently part of the libvirt group 
INFO Checking if libvirt daemon is running        
INFO Checking if a supported libvirt version is installed 
INFO Checking if crc-driver-libvirt is installed  
INFO Installing crc-driver-libvirt                expose
INFO Checking crc daemon systemd service          
INFO Setting up crc daemon systemd service        
INFO Checking crc daemon systemd socket units     
INFO Setting up crc daemon systemd socket units   
INFO Checking if AppArmor is configured           
INFO Updating AppArmor configuration              
INFO Using root access: Updating AppArmor configuration 
[sudo] password for jegan: 
INFO Using root access: Changing permissions for /etc/apparmor.d/libvirt/TEMPLATE.qemu to 644  
INFO Checking if systemd-networkd is running      
INFO Checking if NetworkManager is installed      
INFO Checking if NetworkManager service is running 
INFO Checking if dnsmasq configurations file exist for NetworkManager 
INFO Checking if the systemd-resolved service is running 
INFO Checking if /etc/NetworkManager/dispatcher.d/99-crc.sh exists 
INFO Writing NetworkManager dispatcher file for crc 
INFO Using root access: Writing NetworkManager configuration to /etc/NetworkManager/dispatcher.d/99-crc.sh 
INFO Using root access: Changing permissions for /etc/NetworkManager/dispatcher.d/99-crc.sh to 755  
INFO Using root access: Executing systemctl daemon-reload command 
INFO Using root access: Executing systemctl reload NetworkManager 
INFO Checking if libvirt 'crc' network is available 
INFO Setting up libvirt 'crc' network             
INFO Checking if libvirt 'crc' network is active  
INFO Starting libvirt 'crc' network               
INFO Checking if CRC bundle is extracted in '$HOME/.crc' 
INFO Checking if /home/jegan/.crc/cache/crc_libvirt_4.9.12.crcbundle exists 
INFO Extracting bundle from the CRC executable    
INFO Ensuring directory /home/jegan/.crc/cache exists 
INFO Extracting embedded bundle crc_libvirt_4.9.12.crcbu:moneybag:ndle to /home/jegan/.crc/cache 
INFO Uncompressing crc_libvirt_4.9.12.crcbundle   
crc.qcow2: 11.69 GiB / 11.69 GiB [------------------------------------------------------] 100.00%
oc: 117.16 MiB / 117.16 MiB [-----------------------------------------------------------] 100.00%
Your system is correctly setup for using CodeReady Containers, you can now run 'crc start' to start the OpenShift cluster
</pre>

##### ℹ️ Starting your local CRC OpenShift Cluster
:x:  Please don't attempt this in our training lab.

```
./crc start
```
The expected output is
<pre>
[jegan@tektutor crc-linux-1.38.0-amd64]$ <b>./crc start</b>
INFO Checking if running as non-root              
INFO Checking if running inside WSL2              
INFO Checking if crc-admin-helper executable is cached 
INFO Checking for obsolete admin-helper executable 
INFO Checking if running on a supported CPU architecture 
INFO Checking minimum RAM requirements            
INFO Checking if crc executable symlink exists    
INFO Checking if Virtualization is enabled        
INFO Checking if KVM is enabled                   
INFO Checking if libvirt is installed             
INFO Checking if user is part of libvirt group    
INFO Checking if active user/process is currently part of the libvirt group 
INFO Checking if libvirt daemon is running        
INFO Checking if a supported libvirt version is installed 
INFO Checking if crc-driver-libvirt is installed  
INFO Checking crc daemon systemd socket units     
INFO Checking if systemd-networkd is running      
INFO Checking if NetworkManager is installed      
INFO Checking if NetworkManager service is running 
INFO Checking if /etc/NetworkManager/conf.d/crc-nm-dnsmasq.conf exists 
INFO Checking if /etc/NetworkManager/dnsmasq.d/crc.conf exists 
INFO Checking if libvirt 'crc' network is available 
INFO Checking if libvirt 'crc' network is active  
INFO Starting CodeReady Containers VM for OpenShift 4.9.12... 
INFO CodeReady Containers instance is running with IP 192.168.130.11 
INFO CodeReady Containers VM is running           
INFO Check internal and public DNS query...       
INFO Check DNS query from host...                 
INFO Verifying validity of the kubelet certificates... 
INFO Starting OpenShift kubelet service           
INFO Waiting for kube-apiserver availability... [takes around 2min] 
INFO Waiting for user's pull secret part of instance disk... 
INFO Starting OpenShift cluster... [waiting for the cluster to stabilize] 
INFO All operators are available. Ensuring stability... 
INFO Operators are stable (2/3)...                
INFO Operators are stable (3/3)...                
INFO Adding crc-admin and crc-developer contexts to kubeconfig... 
Started the OpenShift cluster.
penShift Installer Provisioned Ins
The server is accessible via web console at:
  https://console-openshift-console.apps-crc.testing

Log in as administrator:
  Username: kubeadmin
  Password: B8XxM-aY9yz-zhwJY-5HU7d

Log in as user:expose
  Username: developer
  Password: developer

Use the 'oc' command line interface:
  $ eval $(crc oc-env)
  $ oc login -u developer https://api.crc.testing:6443
[jegan@tektutor crc-linux-1.38.0-amd64]$ 
</pre>
when the crc prompts for pull secret, you need to paste the content of pull-secret.txt and hit enter.

##### ℹ️ Troubleshooting CRC start
:x:  Please don't attempt this in our training lab.

It is commonly noticed that ./crc start command fails many times. 

Make sure

1. Virtualization is enabled (VT-X/AMD-V)
2. You have sufficient RAM in the system atleast 16GB or more
3. You have alteast 8 vCPU in your system

Try to stop and startAgreement

```
./crc stop
./crc start
```

##### ℹ️ Login to CRC Cluster as a developer via CLI
:x:  Please don't attempt this in our training lab.

```
eval $(./crc oc-env)
oc login -u developer https://api.crc.testing:6443
```

##### ℹ️ Login to CRC Cluster as an administrator via CLI
:x:  Please don't attempt this in our training lab.

```
eval $(./crc oc-env)
oc login -u kubeadmin https://api.crc.testing:6443
```

## ℹ️ Using RedHat OpenShift Developer Sandbox for Free 👨‍🎓

🔴🔴🔴 As we already have a working OpenShift Cluster pre-installed, you don't have to do this during the training.🔴🔴🔴

You may setup a RedHat Cluster almost instantaneously and very helpful for self-learning.  However, some advanced features like Eventing, installing Operators, etc will not work in this Free environment.

https://developers.redhat.com/developer-sandbox?source=sso

## ℹ️ OpenShift Installer Provisioned Infrastructure (IPI) 💲💲💲
This mode of OpenShift installation is preferred if budget is not a constraint and you wish to perform the OpenShift installation in Cloud environments or with VMWare vSphere, etc.,

However, this style of Openshift installation offers less flexibility or configuration options but the installation efforts will be less, as pretty much the installer automates creating infrastructure (VMs, Network, OS installation, etc) and end-to-end OpenShift installation procedure.

### ℹ️ Story time - my personal experience
I recently installed RedHat OpenShift in AWS. 

I used the OpenShift cluster for 9 days

The automatic installation spinned off 

On Demand Linux Amazon Elastic Compute Cloud running Linux/Unix costed - $420
- EC2 m5.xlarge Instance ( 4 vCPU with 16 GB RAM ) 
- EC2 r5.xlarge Instance ( 4 vCPU with 32 GB RAM - good for memory intensive computing )
- EC2 m5.2xlarge ( 8 vCPU with 32 GB RAM )

Amazon NAT Gateway costed $94 
Load Balancing - $20 

The total bill was :heavy_dollar_sign::heavy_dollar_sign: $707(rounded) :angry: including :disappointed: :disappointed: GST $107.83 for 9 Days. This is way too expensive for your learning purpose, this is more suitable for :heavy_check_mark: corporates :moneybag: :heavy_dollar_sign: i.e Development :heavy_check_mark: & Production :heavy_check_mark:.

 :-1: Not recommended for self-learning

You may refer more details about this in the official documentation
https://docs.openshift.com/container-platform/4.9/installing/index.html

## :heavy_check_mark: OpenShift User Provisioned Infrastructure (UPI)
This mode of OpenShift installation is highly preferred :thumbsup: and flexible :satisfied: in many cases. This approach let's you take control of the installation process, allows you to customize things giving more flexibily in setting up your OpenShift in your own preferred way. 

But this involves doing everything manually yourself :weary: .  This is a very lengthy process and many things can go wrong during the installation, hence installing OpenShift using this approach involves several attempts but the end result almost always will be fruitful as you would have learned many things along the way :v:

I remember the first time I attempted this it took about 7 days :tired_face: to get my cluster up and running. The official documentations are very exhaustive and no single approach works in all environment, hence there are always many missing pieces of information without which things won't work in your environment. You need to find your solution for the puzzle all by yourself.  Sometimes it takes few minutes, few hours and sometimes it takes few days to find out the missing piece of information. It is like solving a puzzle. But once you have figured out what works in your environment, you can do the setup within 1 hour.

## ℹ️ Understanding our Training Lab OpenShift Setup
Our Training Lab is setup using User Provisioned Infrastructure, almost everything was performed manually.

Lab Server Hardware Configuration
<pre>
Dell PowerEdge R630 
Processor Type: Intel(R) Xeon(R) CPU E5-2683 v4 @ 2.10Ghz
  - Logical Processors: 64
NICs : 4
512 GB RAM
6 TB HDD
</pre>
The Server with this kind of configuration might cost you Rs.10,00,000+ 

We have used two such servers to setup our Lab with two OpenShift Clusters.

You may refer the official documentation for detailed installation instructions 
https://docs.openshift.com/container-platform/4.9/installing/index.html

- RedHat CentOS 7.9 64-bit OS is installed on the Server directly as base OS.
- Within RedHat CentOS 7.9, KVM Opensource Hypervisor is installed for Virtualization
- Using KVM ( Kernal-based Virtual Machine ) 6 Virtual Machines were created within RedHat CentOS v7.9
     - 3 master virtual machines are created to setup the OpenShift cluster with 3 Master Nodes
     - 2 worker Virtual machines are created to setup the OpenShift cluster with 2 Worker Nodes
     - 1 LoadBalancer virtual machines with HAProxy is setup to make the cluster accessible.  

- Master Node Virtual Machine Hardware Configuration (RedHat Enterprise Linux Core OS - v49.84.202202081504-0 [Ootpa])
  - 8 Virtual Cores
  - 64 GB RAM
  - 500 GB HDD Storage

- Worker Node Virtual Machine Hardware Configuration (RedHat Enterprise Linux Core OS - v49.84.202202081504-0[Ootpa])
  - 8 Virtual Cores
  - 64 GB RAM
  - 500 GB HDD Storage

- LoadBalancer (HAProxy - Centos 7 cloud image: CentOS-7-x86_64-GenericCloud.qcow2)
  - 2 Virtual Cores
  - 16 GB RAM
  - 100 GB HDD Storage

<pre>
As part of RedHat Enterprise Core OS, we get kubelet and CRI-O container runtime out of the box on all master and worker nodes.
Our OpenShift version is 4.9.21 which uses Kubernetes v1.22.3.x
There are 2 such separate OpenShift clusters setup for our training lab.

Each OpenShift cluster supports upto 10 users.
OpenShift Cluster - 1 ( 8 Users  )
   - Server 1 ( 192.168.1.237 )
OpenShift Cluster - 2 ( 8 users )
   - Server 2 ( 192.168.1.238 )
</pre>
