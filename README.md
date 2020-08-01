# 2020 guide to working Free AWS PySpark instance
   Hello, the purpose of this notebook is to help every aspiring data scientist, data analyst, data engineer, and even individuals in who are just curious how to brush up on Spark when they have a low-powered machine with very limited resources in the year 2020. 

In this example we will be setting up pySpark using EC2 instances and remaining in the "free tier" in AWS. You an use this cluster to write code using a small sample of data then apply to the full dataset, use it to follow along with a class, or learn another interesting way to spin up Spark clusters other than EMR, DataBricks, or local instances to be used if you only have a tablet or Chromebook handy.

The security setting we will choose are rather low, but you can start and stop your server as you see fit and this way you can login from anywhere in the world and start your coding. No more excuses about not being able to study because you have no access to powerful machines.

The very first thing you need to do is setup an AWS account and provide your credit card details. Below is a link to help start creating your fre AWS account.

    https://aws.amazon.com/free
    
### Begin setting up your free pySpark cluster in AWS

#### Here is our plan:
   * Create EC2 instance on ASW
   * Use SSH (Secure Shell Connetion) to connect to EC2 over internet (Windows) with a nice bonus at the end
   * Set-up Spark and Jupyter on EC2 Instance
   
I chose to use Windows instead of Mac or Linux becasue SSH with those is only 2 simple command lines, but Windows has enough steps to make it complicated for new users without help the first time.

We will start with the consol which is what you will see once you log into AWS.

#### First we will select ECR and begin launching an instance

Navigate to EC2 from the consol

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/1_navigateToEC2.gif?raw=true" width="700"/>
</div>

From here we will select "Ubuntu Server 18.04 LTS (HVM), SSD Volume Type" and make sure it says "Free tier eligible"

We will select the "General Purpose" instance "t2.micro" which is the smallest instance offering 1 core and 1GB memory. The best part of the free tier is you get 720-750 free hours per month of free tier EC2 instances, but none in EMR which is why we are using EC2 instead.

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/2_selectYourInstances.gif?raw=true" width="700"/>
</div>

Configuring our instance to remain in the free tier means we will only select QTY 1 node. The purpose of this instance is to work with small datasets for practice. If you need more compute I suggest you use EMR, but this will take you out of free tier.

You will get 8GB of storage in this instance free as an EBS volume (storage dedicated to your instance like a hard drive).

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/3_numNodesAndStorage.gif?raw=true" width="700"/>
</div>

#### Next we will add our tags and configure a security group:

•	Here the key is '"myspark" and the value is "mymachine" and you should make note of this for later. If you choose to edit security groups later, but we will not inthis tutorial.

#### Next you want to make sure "Crete new security group" is selected and change the type to "All Traffic"

•	You will see a security warning here, but you want to easily access your EC2 instance. If you are on a company platform or need additional security you will want to add additional security to your instance.

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/4_tagsAndSecurity.gif?raw=true" width="700"/>
</div>

Select "Launch" in the lower right corner and you need to select your key pair which will allow you access to your EC2 instance. It is **VERY IMPORTANT** to select "Download Key Pair" which will give you a .pem file. Do not lose this file or you will need to start all over again. You cannot get a new key pair for this instance. Select "Launch instance."

Proceed to the next step as your instance is launching.

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/5_keyPairDownload.gif?raw=true" width="700"/>
</div>

### Setting up PUTTY on Windows to SSH into your EC2 Instance

#### Navigate to the link below for full PUTTY documentation. I will be performing these steps below. 

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html

#### Here is the link you will need to access the downloads:

https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

We will install 2 programs from this page. These are links to the 64-bit versions so if you need the 32-bit versions then select the link above and choose those files.

* putty.exe
https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe

* puttygen.exe
https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe

Move these files to your desktop along with the .pem file as we will use them shortly.

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/6_navigateToPUTTY.gif?raw=true" width="700"/>
</div>

We need some information about our instance which we will gather over the next few steps.

We need:
* ID of our instance (so you know which instance belongs to this tutorial)
* Public DNS name
* Our private key (remember the .pem file we downlaoded)

We also need to enable SSH, but we did this when we selected "all inbound traffic," but you could have just opened port 22 after launching your instance. For the sake of practise we kept it simple.

#### Covert the .pem file into .ppk with puttygen.exe for windows to use

Windows does not use .pem files, so we need to change it to .ppk which windows does use.

When you open puttygen.exe the .pem file will not readily show, so select "all files" in your windows explorer. Remember I put all of the files I need onto my desktop for ease of presentation (putty.exe, puttygen.exe, and newspark.pem)

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/7_getPPK.gif?raw=true" width="700"/>
</div>

#### We are now going to start a putty session using that .ppk file we just created as our private key.

We need our DNS name

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/8_DNSname.gif?raw=true" width="700"/>
</div>

We need our instance ID

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/9_isntanceID..gif?raw=true" width="700"/>
</div>

We need to add our .ppk file to putty. 

- Open putty.exe, under the navigation pane (it says "Category:") select **Connection -> SSH -> Auth**

- Under "**Private key file for authentication:**" select browse and find your .ppk file we just made. It is located on my desktop as this is where I had the .pem file.

We will also be entering the Information from our EC2 Instance
* Be sure to type "ubuntu@" then paste your DNS name into putty.

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/10_SSHin(1)..gif?raw=true" width="700"/>
</div>

You are now SSH'ed into your Ubuntu server in AWS. 

The server that pops up is ready to use. If you typed "python3" then "print('Hello')" you can execute this. You can then type "quit()" to exit python.

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/11_pythonExample.gif?raw=true" width="700"/>
</div>

### Installations on EC2 Ubuntu Server

As cool as it is to already have python running, the goal here is to have Spark running in a Jupyter Notebook. Spark has the capability to use multithreading, near infinite compute, memory, and storage capacity (as long as you have more nodes to give it), and can query your data using SQL.

#### In this portion of the exercise we will:

* Install Spark on EC2
* Install Jupyter Notebook on EC2
* Open our notebook through the browser using port 8888

#### Run the following commands on your server in this order, one at a time

##### Update our server OS

    sudo apt-get update

#####  Install pip (hit y for "yes" when prompted)

    sudo apt install python3-pip

##### Install Jupyter Notebook 

    sudo apt install jupyter-notebook 

##### Install Java, incase it is not already installed (hit y for "yes" when prompted)
(know that spark is written in scala, which is written in java, so making sure you have java is essential)

    sudo apt-get install default-jre

##### Install Scala (hit y for "yes" when prompted)

    sudo apt-get install scala

##### Install py4J library
    
    pip3 install py4j

##### Install Spark

    wget http://archive.apache.org/dist/spark/spark-2.1.1/spark-2.1.1-bin-hadoop2.7.tgz

##### Unzip the .tgz file
    
    sudo tar -zxvf spark-2.1.1-bin-hadoop2.7.tgz
    
##### cd into the spark folder 

    cd spark-2.1.1-bin-hadoop2.7/

##### type "pwd" and remember your "/home/ubuntu/spark-2.1.1-bin-hadoop2.7" file path

    pwd

##### cd back to your home directory

    cd

##### Install findspark module to help connect python with spark

    pip3 install findspark
    
##### make .pem files we will use for our Jupyter configuration file

    jupyter notebook --generate-config
    
    mkdir certs
    
    cd certs
    
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
    
**(You will hit a part where the server is asking you for information. You can fill this out or you can keep hitting enter and leave it blank until you reach your command line again. I personally do not mind filling in my location data.)**

##### Edit the configuration file you made 

    cd ~/.jupyter/

    vi jupyter_notebook_config.py
    
#### Here is where it gets tricky. 

You now have open your jupyter notebook configuration file. We need to edit the configuration file. Follow these instructions carefully.

1) press "i" on your keyboard

2) Arrow to under the first line "# Configuration file for jupyter notebook" and add these lines into the config file. The open_browser=False command will prevent the notebook from opening in the browser by default on start.

* c = get_config()
* c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem'
* c.NotebookApp.ip = '*'
* c.NotebookApp.open_browser = False
* c.NotebookApp.port = 8888

3) press "ESC" on your keybaord

4) type ":wq!"

5) hit "Enter"

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/12_configFile.gif?raw=true" width="700"/>
</div>

#### Last Step before starting Jupyter Notebook

In order to connect to your instance with "EC2 Instance Connect (browser-based SSH connection)" you need to install ec2-instance-connect to your AMI.

    cd
    
    sudo apt-get install ec2-instance-connect 

Type the code below to return to your home directory and start your process of logging into your personal server

    jupyter notebook
    
The output message will say "Copy/paste this URL into your browser when you connect for the first time." the token which was generated for me is below. (since we are using putty, but highlight the token and it will save to your clipboard. If you do ctrl+c then it will ask of you want to terminate the server, so do not do this). Also, your token will change every time so I included an easier way to access your token to login below.

#### We need to make changes to this link by adding the DNS address 
Remember we already used our DNS address when logging in with putty, so you should have this available or know where it is at.

replace "localhost" in the url provided with your DNS name and paste into your browser

**my token before**
https://localhost:8888/?token=21182a8fe774db44c7dfba73c56597d9fea9827fe0e90b76

**my token after**
https://ec2-100-26-199-27.compute-1.amazonaws.com:8888/?token=21182a8fe774db44c7dfba73c56597d9fea9827fe0e90b76

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/13_copytoken.gif?raw=true" width="700"/>
</div> 

You will get an error because your browser is considering if the instance is trying to steal your information, which we know it is not. Proceed anyway. There will be an option on your screen under "advanced."

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/14_pasteURL.gif?raw=true" width="700"/>
</div> 

### Configure Jupyter Notebook to use PySpark

Remember installing "findspark"? Now we will need this and the directory you saved from earlier.

#### Run the following Code in your jupyter notebook

    import findspark
    
    findspark.init('/home/ubuntu/spark-2.1.1-bin-hadoop2.7')
    
    import pyspark
    
    from pyspark.sql import SparkSession
    
    spark = SparkSession.builder.appName('FromMattsTutorial').getOrCreate()
    
    spark.version  
    
You are now able to type and use PySpark on your own personal EC2 instance. You can upload datasets using the directory which initially pops up when we opened a new notebook session. You will want to save these lines of code as you will always need to start a new spark session each time anyway, but feel free to use this FREE server anytime to practice python3 OR PySpark OR just make a cool markdown for your side interests.

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/15_StartSparkSession.gif?raw=true" width="700"/>
</div> 

#### Close down your Jupyter Notebook since we were liberal with our permissions

There are enough free hours where you can leave this running 29 days out of the month with guaranteed $0 charges if it is the only free instance running.

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/17_ShutDown.gif?raw=true" width="700"/>
</div> 

### Another easier way to get access your token 

Here we use the direct connect feature, which means you no longer need putty.

<div>
<img src="https://github.com/MatSalm/PortfolioDemonstration/blob/master/16_connectButton.gif?raw=true" width="700"/>
</div> 

# For those who do not mind paying a little extra (about $5 per 24 hours of runtime) for a basic 8-core , 32GB, Pyspark Multithreaded Experience in 5 minutes

**Click the link below**

You shuold terminate this EMR cluster after each use. It is super easy to access, super easy to spin up, and you can connect to an existing S3 bucket where your keep you datasets.

I will be opening port 8890 and 22, but they will need to be closed before spinning up your next cluster and re-opened to prevent your cluster from self terminating (unless you configure a security group). 

This is a full video with great quality and limited explanation. It is that easy.

Please notice I type ":8890" after the DNS name to begin using the zeppelin notebook

You will need to execute this code at the begining of each session and include "%pyspark" which will automatically appear in each code chunk from now on.
    
    %pyspark
    
    from pyspark.sql import SparkSession
    
    spark = SparkSession.builder.appName('mySessionName').getOrCreate()

[![AWS EMR](https://i.imgur.com/StuJUE7.png)](https://www.youtube.com/watch?v=RVReQHo7LEM "AWS EMR")
