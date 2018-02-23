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
  * `INFLUX_DBASE_HOST= <IP of machine running influxdb>`
* components/config.py
  * `DSO_ADDRESS = 'tcp://<IP of machine running DSO>:10001'`
  * `DATA_PATH = <path to repo>/test-10-11-withbattery/opal-data10x/`
* launcher.sh
  * `MINER=<IP of machine running MINER>`  
* test-10-11-withbattery/testrun.sh
  * `nohup python3 $PARENT/components/SmartHomeTraderWrapper.py $i`**`<IP of machine running MINER>`**`10000 ...`
* miner/launchBC
  * tmux send -t miner.0 "$DIR/geth-linux-amd64/geth --datadir $DIR/eth --rpc --rpcport 10000 --rpcaddr **`<IP of machine running MINER>`** --nodiscover --rpcapi "eth,web3,admin,miner,net,db" --password password.txt --unlock 0 --networkid 15 --mine | tee miner.out" ENTER


  
  
## How to run
  1. Start the miner
    1. `cd EnergyMarket/miner`
    2. `./launchBC.sh`
  1. Start the DSO, Solver, and Event recorder
    1. `cd EnergyMarket/`  
      1. `launcher.sh`
  1. Start the prosumers
     1. `cd EnergyMarket/`
     1. `./test-10-11-withbattery/testrun.sh`
     * or to run from terminal
     * `nohup python3 components/SmartHomeTraderWrapper.py <prosumer id> <MINER IP> 10000`
  1. Open Grafana
    1. open browser
    1. http://localhost:3000
    1. Click spiral icon in top left corner
    1. select `Dashboards` -> `Import`
    1. Upload Energy-Market/Grafana-dashboard/Energy Market Local-1519239580453.json
    1. Ajust time range to fit data
        * click clock in top right corner. 
        
## How to see output
  * Miner
     * `tmux attach -t miner.0`
  * DSO/Recorder/Solver
    * `tail -f logs/DSO.out`
      * Typically open this one to see when the contract has been deployed
        * ```2018-02-22 08:38:36,370 / INFO: Deploying contract...
           2018-02-22 08:38:36,430 / INFO: Transaction receipt: 0x74d2f36bb576c82ae0508e00a4bd843019726b24091ef99a1b86783ab034a54e
           2018-02-22 08:38:41,452 / INFO: Waiting for contract to be mined... (block number: 0x6a)
           2018-02-22 08:38:41,493 / INFO: Created filter (ID = 0x56c273862279920f83d71285eb3660f4).
           2018-02-22 08:38:41,700 / INFO: Contract address: 0x4fd5dd705c63811b63b6bef24224b778972fa9df
           2018-02-22 08:38:41,700 / INFO: Entering main function...
           2018-02-22 08:38:41,718 / INFO: Finalizer thread starting...
           2018-02-22 08:38:41,718 / INFO: START_INTERVAL 28
           2018-02-22 08:38:41,718 / INFO: time 1519310321.718452
           2018-02-22 08:38:41,718 / INFO: epoch 1519306961.718452
           2018-02-22 08:38:41,718 / INFO: Listening for traders and solvers...```
    * `tail -f logs/Recorder.out`
    * `tail -f logs/Solver.out`
  * Prosumer
    * `tail -f test-10-11-withbattery/prosumer101.out `

    
     

