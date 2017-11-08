#Make sure you have run 'vagrant plugin install vagrant-vbguest' first.
Vagrant.configure("2") do |config|
	config.vm.define "master" do |master|
	master.vm.provider :virtualbox
	#Jenkins Master node running on Centos 7
	master.vm.box = "centos/7"
	master.vm.hostname = "master.mylab.local"
	master.vm.network :private_network, ip:"10.0.0.3"
	#master.hostmanager.aliases = %w(master1)
	config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
	master.vm.provision "shell", inline: <<-SHELL
		
		sudo yum install epel-release -y
		sudo yum update -y
		#Java installtion and path set
		sudo yum install java-1.8.0-openjdk.x86_64 -y
		java -version
		sudo cp /etc/profile /etc/profile_backup
		echo 'export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk' | sudo tee -a /etc/profile
		echo 'export JRE_HOME=/usr/lib/jvm/jre' | sudo tee -a /etc/profile_backup
		source /etc/profile
		#cross check Java path and home dir
		echo $JAVA_HOME
		echo $JRE_HOME
		#Installation of wget
		sudo yum install wget -y
		#Installation of Jenkins
		cd ~
		sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
		sudo rpm --import http://pkg.jenkins-ci.org/redhat-stable/jenkins-ci.org.key
		sudo yum install jenkins -y
		#Start the Jenkin service 
		sudo systemctl start jenkins.service
		sudo systemctl enable jenkins.service
		#Open the 8080 port to access the Jenkins
		sudo systemctl start jenkins.service
		sudo systemctl enable jenkins.service
		#Allow Jenkins port on firewall
		sudo firewall-cmd --zone=public --permanent --add-port=8080/tcp
		sudo firewall-cmd --reload
		#Installation of git 
		sudo yum install git -y
		#Download The Maven
		cd /opt
		sudo wget http://www-eu.apache.org/dist/maven/maven-3/3.5.2/binaries/apache-maven-3.5.2-bin.tar.gz
		sudo tar -xvf apache-maven-3.5.2-bin.tar.gz
		sudo sed -i '/PasswordAuthentication/d/' /etc/ssh/sshd_config
		sudo echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config
		sudo systemctl reload  sshd
		SHELL
	end
end

