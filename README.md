## Kubernetes IPFS Kubo Cluster

Prepare Configuration Values
Configuring the Secret Resource
In Kubernetes, Secret objects are used to hold values such as tokens, or private keys.

```bash
#### secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-config
type: Opaque
data:
  cluster-secret: <INSERT_SECRET>
```

### Cluster Secret

To generate the cluster_secret value in secret.yaml, run the following and insert the output in the appropriate place in the secret.yaml file:

```bash
$ od  -vN 32 -An -tx1 /dev/urandom | tr -d ' \n' | base64 -w 0 -
```

Bootstrap Peer ID and Private Key
To generate the values for `bootstrap_peer_id` and `bootstrap_peer_priv_key`, install ipfs-key and then run the following:

```bash
$ ipfs-key | base64 -w 0
```

Copy the id into the `env-configmap.yaml` file. I.e:


```bash
# env-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: env-config
data:
  bootstrap-peer-id: <INSERT_PEER_ID>
```

Then copy the private key value and run the following with it:

```bash
$ echo "<INSERT_PRIV_KEY_VALUE_HERE>" | base64 -w 0 -
```

Copy the output to the `secret.yaml` file.

```bash
# secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-config
type: Opaque
data:
  cluster-secret: <INSERT_SECRET>
  bootstrap-peer-priv-key: <INSERT_KEY>
```

Execute : 
```bash 
k apply -f .
```

Delete :
```bash
k delete -f .
```