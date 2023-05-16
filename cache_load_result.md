----------------------------------------------------------------------------------------
POSTGRESQL
----------------------------------------------------------------------------------------
~$ wrk -d 60 -t 10 -c 10 --latency -s /mnt/c/ttt/get.lua http://192.168.1.49:8081
Running 1m test @ http://192.168.1.49:8081
  10 threads and 10 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    32.41ms   42.40ms 489.72ms   97.07%
    Req/Sec    38.32      7.47    50.00     63.18%
  Latency Distribution
     50%   24.85ms
     75%   28.02ms
     90%   34.19ms
     99%  301.23ms
  21734 requests in 1.00m, 6.19MB read
  Socket errors: connect 0, read 0, write 0, timeout 10
Requests/sec:    361.86
Transfer/sec:    105.59KB


~$ wrk -d 60 -t 10 -c 10 --latency -s /mnt/c/ttt/get.lua http://192.168.1.49:8082
Running 1m test @ http://192.168.1.49:8082
  10 threads and 10 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    43.13ms  188.57ms   1.92s    96.88%
    Req/Sec    81.12     14.47   121.00     81.76%
  Latency Distribution
     50%   11.74ms
     75%   13.31ms
     90%   16.89ms
     99%    1.28s
  44774 requests in 1.00m, 12.68MB read
  Socket errors: connect 0, read 0, write 0, timeout 10
Requests/sec:    745.50
Transfer/sec:    216.22KB

----------------------------------------------------------------------------------------
MONGODB
----------------------------------------------------------------------------------------
~$ wrk -d 60 -t 10 -c 10 --latency -s /mnt/c/ttt/get.lua http://192.168.1.49:8081
Running 1m test @ http://192.168.1.49:8081
  10 threads and 10 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     8.86ms    1.70ms  30.29ms   75.60%
    Req/Sec   113.25      8.76   141.00     79.13%
  Latency Distribution
     50%    8.68ms
     75%    9.68ms
     90%   10.87ms
     99%   14.19ms
  67707 requests in 1.00m, 15.73MB read
Requests/sec:   1127.59
Transfer/sec:    268.24KB

~$ wrk -d 60 -t 10 -c 10 --latency -s /mnt/c/ttt/get.lua http://192.168.1.49:8082
Running 1m test @ http://192.168.1.49:8082
  10 threads and 10 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     5.15ms    1.19ms  34.74ms   81.08%
    Req/Sec   195.18     14.80   272.00     61.57%
  Latency Distribution
     50%    5.01ms
     75%    5.63ms
     90%    6.31ms
     99%    9.16ms
  116732 requests in 1.00m, 27.11MB read
Requests/sec:   1943.99
Transfer/sec:    462.27KB

----------------------------------------------------------------------------------------
ВЫВОД
----------------------------------------------------------------------------------------
Монго даёт кратный прирост скорости чтения данных