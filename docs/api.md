# API mailcow

- Thêm add cho domain

    + url: `/api/v1/add/domain-admin`
    + attr: `'{"username":"minhkma","domains":"vietkubers.vn","password":"*","password2":"*","active":"1"}'`
    + methol: POST

- Thêm domain cho 

    + url: `/api/v1/add/domain`
    + attr: `'{"domain":"vietkubers.vn","description":"test add domain","aliases":"20","mailboxes":"20","maxquota":"3072","quota":"10240","active":"1"}'`

- Thêm mail-box:

    + url: `/api/v1/add/mailbox`
    + attr: `'{"local_part":"admin","domain":"vietkubers.vn","name":"Nguyen Van Minh","quota":"3072","password":"*","password2":"*","active":"1"}'`
    + methol: POST

## ex:

```
curl -X POST https://mail.vietkubers.vn/api/v1/add/domain -d attr='{"domain":"vietkubers.vn","description":"test add domain","aliases":"20","mailboxes":"20","maxquota":"3072","quota":"10240","active":"1"}' -H "X-API-Key: CBEA50-3E417D-EEF53A-F4C3F7-6C3987"
```
