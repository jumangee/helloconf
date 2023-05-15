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