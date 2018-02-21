# Energy-Market

## Prerequisites
Install Grafana http://docs.grafana.org/installation/debian/
  1. `wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.6.3_amd64.deb`
  1. `sudo apt-get install -y adduser libfontconfig`
  1. `sudo dpkg -i grafana_4.6.3_amd64.deb`
  1. `sudo systemctl enable grafana-server.service`
  1. `sudo systemctl start grafana-server`
  1. `systemctl status grafana-server`

Install Influxdb https://docs.influxdata.com/influxdb/v1.4/introduction/installation/
  1. `curl -sL https://repos.influxdata.com/influxdb.key | sudo apt-key add -
      source /etc/lsb-release
      echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/influxdb.list`
  1. `sudo apt-get update && sudo apt-get install influxdb`
  1. `sudo systemctl start influxdb`
  
Install tmux
  1. `sudo apt install tmux`


## Set config files
* components/Grafana/config.py
  * INFLUX_DBASE_HOST= \<IP of machine running influxdb>
* components/config.py
  * DSO_ADDRESS = 'tcp://\<IP of machine running DSO>:10001'
* launcher.sh
  * MINER=\<IP of machine running MINER>  
* test-10-11-withbattery/testrun.sh
  * nohup python3 $PARENT/components/SmartHomeTraderWrapper.py $i **\<IP of machine running MINER>** 10000 ...
  
  
## How to run
  1. Start the miner
    1. cd EnergyMarket/miner
    2. ./launchBC.sh
  1. Start the DSO, Solver, and Event recorder
    1. cd EnergyMarket/
      1. launcher.sh
  1. Start the prosumers
     1. cd EnergyMarket/
     1. ./test-10-11-withbattery/testrun.sh
     * or to run from terminal
     * nohup python3 components/SmartHomeTraderWrapper.py \<prosumer id> \<MINER IP> 10000
  1. Open Grafana
    1. open browser
    1. http://localhost:3000
    1. Click spiral icon in top left corner
    1. select `Dashboards` -> `Import`
    1. Upload Energy-Market/Grafana-dashboard/Energy Market Local-1519239580453.json
    1. Ajust time range to fit data
        * click clock in top right corner. 

    
     
