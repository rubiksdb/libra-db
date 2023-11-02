# what is libra-db
  Libra is a key-value in-memory DB.  Any change (commit) goes through
  paxos logging and replicated n-way.  By replaying the paxos log, we
  can always rebuild the database in-memory.

  Libra has greate API latency due to in-memory property.

# where libra-db can be used
  Libra is a great fit for building a root server with limit number of
  records in memory.

  It is also possible to create mutlple libra db across many servers
  to serve large number of in-memory records.

# run libra db locally
  - get and unpack the package
    ```
    tar xvf libra-db.tgz
    ```

  - bring up the service locally (3-way replication)
    ```
    ./local/run-libra.sh
    ```

  - set endpoint into a env
    ```
    export LIBRA="-l 127.0.0.1:10000 -l 127.0.0.1:20000 -l 127.0.0.1:30000"
    ```

  - query libra extent (aka db)
    ```
    hxu@hxu-XPS-9320:~/libra-db$ go/bin/libra-extent $LIBRA query

    participant_id               endpoint           bits      ecrow
    PARTICIPANT(1,3531507660)    127.0.0.1:10004    PC Q0 Q1  0
    PARTICIPANT(2,2122740104)    127.0.0.1:20004    PC Q0 Q1  1
    PARTICIPANT(3,370602853)     127.0.0.1:30004    PC Q0 Q1  2
    ```

  - performance test
    ```
    xu@hxu-XPS-9320:~/libra-db$ go/bin/libra-perf $LIBRA -wr 1 -ds count=10000
    2023-11-02T23:18:16 libra-perf.go:48/main  INFO Libra performance test with: rr=0 wr=1 offset=0,count=10000,uint
    2023-11-02T23:18:16 libra-em.go:194/lookup  INFO EXTENT(8) master discovered 127.0.0.1:10006 in 688.255µs
    2023-11-02T23:18:16 libra-perf.go:62/func1  INFO write min 296.834µs max 1.735799ms avg 394.467µs
    2023-11-02T23:18:16 libra-perf.go:62/func1  INFO write min 204.678µs max 466.393µs avg 293.03µs
    2023-11-02T23:18:16 libra-perf.go:62/func1  INFO write min 158.323µs max 471.538µs avg 301.887µs
    2023-11-02T23:18:16 libra-perf.go:62/func1  INFO write min 117.305µs max 257.069µs avg 164.929µs
    2023-11-02T23:18:16 libra-perf.go:62/func1  INFO write min 115.573µs max 179.779µs avg 132.397µs
    2023-11-02T23:18:16 libra-perf.go:62/func1  INFO write min 118.768µs max 224.618µs avg 140.311µs
    ...
    2023-11-02T23:18:17 libra-perf.go:62/func1  INFO write min 99.836µs max 278.054µs avg 137.756µs
    2023-11-02T23:18:17 libra-perf.go:62/func1  INFO COMMIT 10000 all done 1.523192597s!!
    2023-11-02T23:18:17 libra-perf.go:68/main  INFO all done!!
    ```

  - list the inserted db
    ```
    hxu@hxu-XPS-9320:~/libra-db$ go/bin/libra-cli $LIBRA list 0
    ...
    ```
