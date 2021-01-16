# s3fuse
Если возникнут проблемы, можно запустить с опцией отладки
s3fs mybucket /path_to_mountpoint -o passwd_file=/root/.passwd-s3fs -d -d -f -o f2 -o curldbg
