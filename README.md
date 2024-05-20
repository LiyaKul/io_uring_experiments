Проверка, что ядерный тред поллит очередь: 
```
$ sudo bpftrace -e 'tracepoint:io_uring:io_uring_submit_sqe {printf("%s(%d)\n", comm, pid);}’
iou-sqp-643685(643685)
iou-sqp-643685(643685)
...
```
Для сравнения, без SQPOLL:
```
$ sudo bpftrace -e 'tracepoint:io_uring:io_uring_submit_sqe {printf("%s(%d)\n", comm, pid);}’
remote-storage-661358(661358)
remote-storage-661358(661358)
...
```
[Trace events](https://elixir.bootlin.com/linux/v5.14-rc6/source/include/trace/events/io_uring.h) для io_uring

Сравнение рпс:
![rps io_uring](https://github.com/LiyaKul/io_uring_experiments/blob/main/rps_1.png)

Flame graph remote_storage:
![rps io_uring](https://github.com/LiyaKul/io_uring_experiments/blob/main/RS_FG.png)

Flame graph remote_storage с io_uring и sqpoll, `io_uring_submit_and_wait(ring, 1)` (дополнительные `sys_enter_io_uring_enter`):
![rps io_uring](https://github.com/LiyaKul/io_uring_experiments/blob/main/URING_FG.png)

Flame graph remote_storage с io_uring и sq_poll, `io_uring_submit_and_wait(ring, 0)`:
![rps io_uring](https://github.com/LiyaKul/io_uring_experiments/blob/main/fg_poll_0.png)


Flame graph remote_storage с io_uring без sq_poll:
![rps io_uring](https://github.com/LiyaKul/io_uring_experiments/blob/main/fg_without_poll.png)
