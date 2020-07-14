# Installation Guide

## System requirements 

### Hardware requirements

|                         	| Requirements                                                     	|
|:-----------------------:	|------------------------------------------------------------------	|
| <b>Operating System</b> 	| Linux Ubuntu 16.04, 18.04<br> Linux CentOS 7+<br> Amazon Linux 2 	|
| <b>       CPU      </b> 	| 8+ Cores                                                         	|
| <b>      Memory    </b> 	| 32 Gb RAM                                                        	|
| <b>    Free space  </b> 	| SSD 128Gb+ of free space                                         	|

> All in one standalone deployment supports up to 5 parallel executors for web and api tests. The most optimal EC2 instance type is t3.2xlarge with enabled "T2/T3 Unlimited" feature

### Software requirements

* Install docker ([Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04), [Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04), [Amazon Linux 2](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html), [Redhat/Cent OS](https://www.cyberciti.biz/faq/install-use-setup-docker-on-rhel7-centos7-linux/) or [MacOS](https://pilsniak.com/how-to-install-docker-on-mac-os-using-brew/))
  > MacOS is <b>not recommended</b> for production usage!
  
* Install [docker-composer](https://docs.docker.com/compose/install/#install-compose)

## Initial setup

1. Clone [qps-infra](https://github.com/qaprosoft/qps-infra)

2. Generate github [OAuth Apps](https://developer.github.com/apps/building-oauth-apps/creating-an-oauth-app/) token and put values into .env.original (GITHUB_CLIENT_ID and GITHUB_CLIENT_SECRET)

3. [Optional] Generate new AUTH_TOKEN_SECRET/CRYPTO_SALT values and put into the variables.env
  > Strongly recommended for publicly available environments! AUTH_TOKEN_SECRET is randomized and base64 encoded string. CRYPTO_SALT is randomized alpha-numeric string

4. [Optional] Disable unused component(s) according to the [steps](#disableremove-components).

5. [Optional] Update default credentials in .env.original
  > Note: due to the huge refactoring better to ask [assistance](https://t.me/qps_infra) in updating credentials for all configuration files. In 5.2 it is planned to have single point of true only inside .env.original!

6. Launch the setup.sh script providing your hostname as an argument:<br>
  ```
  git clone https://github.com/qaprosoft/qps-infra.git && cd qps-infra && ./setup.sh myhost.domain.com
  ```
  > Use public ip address if you don't have registered DNS hostname yet

7. Start services<br>
  ```
  ./start.sh
  ```
  
8. After QPS-Infra startup, the following components are available. Take a look at variables.env for default credentials:
  > Use your host domain address or IP.
  > admin/admin are hardcoded sonarqube credentials, and they can be updated inside the Sonar Administration panel
  
| Components          	| URL                                                	   |
|---------------------	|------------------------------------------------------	 |
| 1st Page            	| http://demo.qaprosoft.com                              |
| Jenkins             	| http://demo.qaprosoft.com/jenkins                      |
| Zebrunner Insights  	| http://demo.qaprosoft.com/app                          |
| SonarQube           	| http://demo.qaprosoft.com/sonarqube                    |
| Web Selenium Hub    	| http://demo:demo@demo.qaprosoft.com/selenoid/wd/hub    |
| Mobile Selenium Hub 	| http://demo:demo@demo.qaprosoft.com/mcloud/wd/hub 	   |
| Smart Test Farm     	| http://demo.qaprosoft.com/stf 	                       |


## Disable/Remove component(s)
QPS-Infra contains such layers of services:

| Layer                        	| Containers                                                           	|
|------------------------------	|----------------------------------------------------------------------	|
| NGiNX WebServer              	| nginx (docker-compose.yml in a root folder)                          	|
| Reporting Services           	| postgres, reporting-service, reporting-ui, zebrunner-proxy etc      	|
| CI (Jenkins)                 	| jenkins-master, jenkins-slave                                       	|
| Code Analysis                	| sonarqube                                                            	|
| MCloud                      	| stf services, selenium-hub, ftp                                      	|
| Embedded web selenium hub    	| selenoid                                                            	|
  
Comment/uncomment in the start.sh (stop.sh and clear.sh) appropriate [line](https://github.com/qaprosoft/qps-infra/blob/82f591827b8cbadd886ea9731661a39aba2ed0a4/start.sh#L20) for the component

## Support Channel

* Join [Telegram channel](https://t.me/qps_infra) in case of any question
