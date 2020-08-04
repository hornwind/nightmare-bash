# nightmare-bash

```
var=$(kubectl -n prod exec db-pg-patroni-0 -- patronictl -c /home/postgres/postgres.yml list -f json | jq -r '.[] | select(.Role == "") | .Host'); sed -i "s/ip: .*/ip: $var/" pgslaveend.yaml; kubectl apply -f pgslaveend.yaml
```

```
source /dev/stdin <<<"$(echo 'cat <<EOF >pgslaveend.yaml'; echo "apiVersion: v1";echo "kind: Endpoints";echo "metadata:";echo " name: pgsqlslave";echo " namespace: prod";echo "subsets:";echo "- addresses:";echo " - ip: 10.244.1.85";echo " ports:";echo " - name: postgresql";echo " port: 5432";echo " protocol: TCP"; echo EOF;)"; var=$(kubectl -n prod exec db-pg-patroni-0 -- patronictl -c /home/postgres/postgres.yml list -f json | jq -r '.[] | select(.Role == "") | .Host'); sed -i "s/ip: .*/ip: $var/" pgslaveend.yaml; kubectl apply -f pgslaveend.yaml
```
