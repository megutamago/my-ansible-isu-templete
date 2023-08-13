# my-ansible-isu-templete

## ISUCON my templete
webapp: private-isu  

<br>

### shell
```
# archive
tar zcvfp /tmp/webapp.tar.gz /home/isucon/private_isu/webapp

# mysqldump
mysqldump -u isuconp isuconp | gzip > /tmp/isuconp.dump.sql.gz 

# scp
scp ubuntu@x.x.x.x:/tmp/webapp.tar.gz ./
scp ubuntu@x.x.x.x:/tmp/isuconp.dump.sql.gz ./
scp ubuntu@x.x.x.x:/etc/nginx/nginx.conf ./

# remove
rm -f /tmp/webapp.tar.gz
rm -f /tmp/isuconp.dump.sql.gz
```

---

### pprof
```
# trace
curl -o trace.out http://localhost:6060/debug/pprof/trace?seconds=60
go tool trace -http=localhost:1080 trace.out

# profile
curl -o cpu.pprof http://localhost:6060/debug/pprof/profile?seconds=60
go tool pprof -http localhost:1080 cpu.pprof
#(go tool pprof -http localhost:1080 cpu.pprof) &   # bg
#kill -9 $(ps aux | grep 'go tool pprof -http localhost:1080 cpu.pprof' | grep -v grep | awk '{print $2}')
ssh -fN -L 0.0.0.0:1080:localhost:1080 192.168.11.41
http://192.168.11.21:1080/
```

---

### alp
```
cat /var/log/nginx/access.log \
| alp ltsv -m '/image/[0-9]+,/posts/[0-9]+,/@\w' \
--sort avg -r -o count,1xx,2xx,3xx,4xx,5xx,min,max,avg,sum,p99,method,uri
```

---

### pt-query-digest
```
cat /var/log/mysql/mysql-slow.sql | grep -v "INSERT INTO \`posts\`" > /var/log/mysql/mysql-slow-tmp.sql
pt-query-digest /var/log/mysql/mysql-slow.sql
```

---

### unarchive
``` 
rm -rf ../test-private_isu/webapp/ && \
tar zxvfp ./files/fetch/webapp.tar.gz -C ../test-private_isu/ && \
mv ../test-private_isu/home/isucon/private_isu/webapp ../test-private_isu/ && \
rm -rf ../test-private_isu/home/
```