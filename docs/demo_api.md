# mailcow demo 


- add domain: 

```
curl -X POST https://mail.supportdao.com/api/v1/add/domain -d attr='{"restart_sogo":"1","domain":"supportdao.com","description":"mail ","aliases":"400","mailboxes":"10","maxquota":"3072","quota":"10240","active":"1","rl_value":"","rl_frame":"s"}' -H "X-API-Key: minhkma"
```

- add dkim: 

```
curl -X POST https://mail.supportdao.com/api/v1/add/dkim -d attr='{"domains":"supportdao.com","dkim_selector":"dkim","key_size":"2048"}' -H "X-API-Key: minhkma"
```

- add user: 

```
curl -X POST https://mail.supportdao.com/api/v1/add/mailbox -d attr='{"local_part":"admin","domain":"supportdao.com","name":"Nguyen Van Minh","quota":"3072","password":"minhnguyen1205","password2":"minhnguyen1205","active":"1"}' -H "X-API-Key: minhkma"
```
