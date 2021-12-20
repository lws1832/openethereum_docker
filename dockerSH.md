version: "3"

services:

  pos1:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8547:8547
      - 30305:30305
      - 10546:10546
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8547
      PORT: 30305
      RUST_BACKTRACE: 1
    volumes:
      - ./config1.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys1:/openethereum/keys
    container_name: pos1
    command: >
      sh -c "cd ~/
            /openethereum/target/release/openethereum --config /openethereum/config.toml"
    working_dir: /openethereum


  pos2:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8548:8548
      - 30306:30306
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8548
      PORT: 30306
      RUST_BACKTRACE: 1
    volumes:
      - ./config2.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys2:/openethereum/keys
    container_name: pos2
    command: >
      sh -c "cd ~/
            /openethereum/target/release/openethereum --config /openethereum/config.toml"
    working_dir: /openethereum

  pos3:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8549:8549
      - 30307:30307
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8549
      PORT: 30307
      RUST_BACKTRACE: 1
    volumes:
      - ./config3.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./keys3:/openethereum/keys
      - ./nodes:/openethereum/nodes
    container_name: pos3
    command: >
      sh -c "cd ~/
            /openethereum/target/release/openethereum --config /openethereum/config.toml"
    working_dir: /openethereum

  pos4:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8550:8550
      - 30308:30308
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8550
      PORT: 30308
      RUST_BACKTRACE: 1
    volumes:
      - ./config4.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys4:/openethereum/keys
    container_name: pos4
    command: >
      sh -c "cd ~/
            /openethereum/target/release/openethereum --config /openethereum/config.toml"
    working_dir: /openethereum
  poscli:
    image: "dndfjddl/openethereum:0"
    tty: true
    container_name: pos-cli
    working_dir: /openethereum
    command: /bin/bash
 
    links:
      - pos1:pos1
      - pos2:pos2
      - pos3:pos3
      - pos4:pos4
    
    depends_on:
      - pos1
      - pos2
      - pos3
      - pos4
  entry:
    image: "nara7875/etcexplorer:0"
    ports:
      - 3000:3000
    depends_on:
      - pos1
      - pos2
      - pos3
      - pos4
      - poscli
      - db
    links:
      - pos1:pos1
    volumes:
      - ./config.json:/config.json
    environment:
      MONGO_URI: mongodb://host.docker.internal/explorerDB
  db:
    image: "mongo"
    ports: 
      - 27017:27017
    links:
      - pos1
    volumes:
      - ./db:/data/db


  