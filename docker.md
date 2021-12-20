version: '3'
services:
  app:
    build: .
    command: ./app.js
    #restart: always
    environment:
      # connection url
      # restart start stop 몽고가 실행이 되었냐.
      # 계정이 생성이 완료가 된상태에서 restart  
      # 생성된 계정으로 도메인주소를 완성시켜서 외부접속을 시도해보기. 
      MONGO_URI: 'mongodb://db/explorerDB'
    ports:
      - '3000:3000'
    depends_on:
      - db

  db:          # service의 이름입니다.
    image: mongo    # 해당 service에서 사용할 image입니다.
    restart: always # container를 실행할 때 항상 이미 수행중이라면 재시작을 수행합니다.
    environment:    # 환경변수를 정의합니다.
      # MONGO_INITDB_ROOT_USERNAME: root
      # MONGO_INITDB_ROOT_PASSWORD: P@ssw0rd
    volumes:        # container -> local로 mount할 수 있습니다.
      - type: bind
        source: ./data/db # local 경로
        target: /data/db  # container 내부에서의 경로
    container_name: "mongodb" # container의 name을 정의합니다.
    ports:                # service port를 정의합니다.
      - "27017:27017"     # local:container

  pos1:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8547:8547
      - 30303:30303
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8547
      PORT: 30303
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
      - 30304:30304
      - 10546:10546
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8548
      PORT: 30304
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
      - 30305:30305
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8549
      PORT: 30305
      RUST_BACKTRACE: 1
    volumes:
      - ./config3.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys3:/openethereum/keys
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
      - 30306:30306
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8550
      PORT: 30306
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
 pos5:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8551:8551
      - 30307:30307
    environment:
      ENV: ETHERNODE5
      RPCPORT: 8551
      PORT: 30307
    volumes:
      - ./config5.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys5:/openethereum/keys
    container_name: pos5
    command: >
      sh -c "cd ~/
            /openethereum/target/release/openethereum --config /openethereum/config.toml"
    working_dir: /openethereum

  pos6:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8552:8552
      - 30308:30308
    environment:
      ENV: ETHERNODE6
      RPCPORT: 8552
      PORT: 30308
    volumes:
      - ./config6.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys6:/openethereum/keys
    container_name: pos6
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
      - pos5:pos5
      - pos6:pos6
      - db:db
      - app:app

    
    depends_on:
        - pos1
        - pos2
        - pos3
        - pos4
        - pos5
        - pos6
        - db
        - app


        -------------------------

version: "2"

services:
  pos1:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8547:8547
      - 30303:30303
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8547
      PORT: 30303
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
      - 30304:30304
    environment:
      ENV: ETHERNODE2
      RPCPORT: 8548
      PORT: 30304
    volumes:
      - ./config2.toml:/ethereum/config.toml
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
      - 30305:30305
    environment:
      ENV: ETHERNODE3
      RPCPORT: 8549
      PORT: 30305
    volumes:
      - ./config3.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys3:/openethereum/keys
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
      - 30306:30306
    environment:
      ENV: ETHERNODE4
      RPCPORT: 8550
      PORT: 30306
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

  pos5:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8551:8551
      - 30307:30307
    environment:
      ENV: ETHERNODE5
      RPCPORT: 8551
      PORT: 30307
    volumes:
      - ./config5.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys5:/openethereum/keys
    container_name: pos5
    command: >
      sh -c "cd ~/
            /openethereum/target/release/openethereum --config /openethereum/config.toml"
    working_dir: /openethereum

  pos6:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8552:8552
      - 30308:30308
    environment:
      ENV: ETHERNODE6
      RPCPORT: 8552
      PORT: 30308
    volumes:
      - ./config6.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys6:/openethereum/keys
    container_name: pos6
    command: >
      sh -c "cd ~/
            /openethereum/target/release/openethereum --config /openethereum/config.toml"
    working_dir: /openethereum

  poscli:
    image: "dndfjddl/openethereum:0"
    tty: true
    container_name: poscli
    working_dir: /openethereum
    command: /bin/bash

    links:
      - pos1:pos1
      - pos2:pos2
      - pos3:pos3
      - pos4:pos4
      - pos5:pos5
      - pos6:pos6

    depends_on:
      - pos1
      - pos2
      - pos3
      - pos4
      - pos5
      - pos6

-------------------------------------------------------------------



version: '3'
services:

  pos1:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8547:8547
      - 30303:30303
      - 10546:10546
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8547
      PORT: 30303
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
      - 30304:30304
      - 10547:10547
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8548
      PORT: 30304
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
      - 30305:30305
      - 10548:10548
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8549
      PORT: 30305
      RUST_BACKTRACE: 1
    volumes:
      - ./config3.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys3:/openethereum/keys
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
      - 30306:30306
      - 10549:10549
    environment:
      ENV: ETHERNODE1
      RPCPORT: 8550
      PORT: 30306
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
  pos5:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8551:8551
      - 30307:30307
      - 10550:10550
    environment:
      ENV: ETHERNODE5
      RPCPORT: 8551
      PORT: 30307
    volumes:
      - ./config5.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys5:/openethereum/keys
    container_name: pos5
    command: >
      sh -c "cd ~/
            /openethereum/target/release/openethereum --config /openethereum/config.toml"
    working_dir: /openethereum

  pos6:
    image: "dndfjddl/openethereum:0"
    tty: true
    ports:
      - 8552:8552
      - 30308:30308
      - 10551:10551
    environment:
      ENV: ETHERNODE6
      RPCPORT: 8552
      PORT: 30308
    volumes:
      - ./config6.toml:/openethereum/config.toml
      - ./genesis.json:/openethereum/genesis.json
      - ./password:/openethereum/password
      - ./nodes:/openethereum/nodes
      - ./keys6:/openethereum/keys
    container_name: pos6
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
      - pos5:pos5
      - pos6:pos6


    
    depends_on:
        - pos1
        - pos2
        - pos3
        - pos4
        - pos5
        - pos6
