# PostgreSQL with PersistentVolume (PV) + PersistentVolumeClaim (PVC)
#
# Flow: Pod -> PVC (request for storage) -> PV (actual storage) -> disk
#
# Usage:
#   kubectl apply -f postgres-pv-pvc.yaml
#   kubectl get pv,pvc -n prod-env           # PVC should show STATUS=Bound
#   kubectl get pods -n prod-env -l app=postgres
#
# NOTE: this PV uses hostPath (a folder on one worker node). It is fine for
# learning/testing. The pod must always run on the same node, and data is lost
# if that node is replaced. For real use on AWS, use an EBS volume
# (see the commented awsElasticBlockStore / CSI example below).
--------- To run ---------
# kubectl apply -f postgres-pv-pvc.yaml
# kubectl get pv,pvc -n prod-env
# kubectl get pods -n prod-env -l app=postgres

--------- To add data and test ---------

# kubectl exec -it deploy/postgres -n prod-env -- psql -U appuser -d appdb -c "create table t(id int); insert into t values (1);"
# kubectl delete pod -l app=postgres -n prod-env
# kubectl exec -it deploy/postgres -n prod-env -- psql -U appuser -d appdb -c "select * from t;"
