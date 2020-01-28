# vault


## Connexion

```bash
export VAULT_ADDR='http://127.0.0.1:8200'
vault login 
vault status
vault secrets list



vault secrets enable kv
vault kv list kv
vault write kv/hello mykey=myValue
```

```bash 
vault -autocomplete-install
vault server -dev
export VAULT_ADDR='http://127.0.0.1:8200'
export VAULT_DEV_ROOT_TOKEN_ID="abcde13455"
vault status
vault secrets list
 
vault kv put secret/hello foo=world excited=yes
```

## Use json

```bash
vault kv get -format=json secret/hello
{
  "request_id": "f63461d9-7c0f-93da-5dda-d8a5169e4961",
  "lease_id": "",
  "lease_duration": 0,
  "renewable": false,
  "data": {
    "data": {
      "excited": "yes",
      "foo": "world"
    },
    "metadata": {
      "created_time": "2019-10-27T16:05:45.259930819Z",
      "deletion_time": "",
      "destroyed": false,
      "version": 1
    }
  },
  "warnings": null
}

vault kv get -format=json secret/hello | jq -r .data.data.excited
yes
vault kv get -format=json secret/hello | jq -r .data.metadata.created_time
2019-10-27T16:05:45.259930819Z
```


## Delete a secret 

```bash
vault kv delete secret/hello
```

## Vault new path

```bash
vault secrets enable -path=kv kv
```

## List secret 
```bash
vault secrets list
Path          Type         Accessor              Description
----          ----         --------              -----------
cubbyhole/    cubbyhole    cubbyhole_acbd3073    per-token private secret storage
identity/     identity     identity_6a079533     identity store
kv/           kv           kv_04cb75a6           n/a
secret/       kv           kv_13ea7ed0           key/value secret storage
sys/          system       system_f1c0ae95       system endpoints used for control, policy and debugging
```

# List a tree 

```bash
vault list kv
Keys
----
my-secret
```

# Write new values

```bash
vault write kv/hello target=world
vault kv/airplane type=boeing class=787
vault write kv/airplane type=boeing class=787
vault list kv
```

# vault disable / enable engine

```bash
vault secrets enable kv/
vault secrets disable kv/
```


# Enable aws Engine type 

```bash
vault secrets enable -path=aws aws
```

# Write aws root

```bash
vault write aws/config/root \
    access_key=AKIAI4SGLQPBX6CSENIQ \
    secret_key=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 \
    region=us-east-1
```

# Write a role

```bash
vault write aws/roles/my-role \
        credential_type=iam_user \
        policy_document=-<<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1426528957000",
      "Effect": "Allow",
      "Action": [
        "ec2:*"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
EOF
```



## Helper 

* https://learn.hashicorp.com/vault/operations/ops-deployment-guide
* /usr/local/bin/vault server -config=/etc/vault.d/vault.hcl



## Secret List
```bash
vault secrets list
Path          Type         Accessor              Description
----          ----         --------              -----------
aws/          aws          aws_1ccf232e          n/a
cubbyhole/    cubbyhole    cubbyhole_c6c95f05    per-token private secret storage
identity/     identity     identity_d2b79ad7     identity store
kv/           kv           kv_78a36871           n/a
secret/       kv           kv_b36c66d1           key/value secret storage
sys/          system       system_ad335977       system endpoints used for control, policy and debugging

vault kv
delete             enable-versioning  list               patch              rollback
destroy            get                metadata           put                undelete
```

## List key/value

```bash
vault kv list kv
Keys
----
test

# Put a key value

```bash
vault kv list secret
Keys
----
mysecret
```

## Put a key/value

```bash
vault kv put secret/dora character1=bobo
Key              Value
---              -----
created_time     2019-10-27T20:27:28.775653501Z
deletion_time    n/a
destroyed        false
version          1
```

## Get value

```bash
vault kv get secret/mysecretlist
```


## Need help

```bash
vault path-help aws
...
...
```

## Create a token

```bash
vault token create
Key                  Value
---                  -----
token                s.D1GU79shONJqF9yexb7JQ5bo
token_accessor       5fiI4a60SERRatuYylpe5iEm
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
``` 


## vault login

```bash
vault login s.D1GU79shONJqF9yexb7JQ5bo
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                s.D1GU79shONJqF9yexb7JQ5bo
token_accessor       5fiI4a60SERRatuYylpe5iEm
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
```

## Revoke a token
vault token revoke s.D1GU79shONJqF9yexb7JQ5bo
Success! Revoked token (if it existed)


## unseal the vault with unseal key

```bash
vault operator unseal
Unseal Key (will be hidden):
```

## get auth list

```bash
vault auth list
Path      Type     Accessor               Description
----      ----     --------               -----------
token/    token    auth_token_779ac454    token based credentials
```

# list policies

```bash
vault policy list
default
root
```



