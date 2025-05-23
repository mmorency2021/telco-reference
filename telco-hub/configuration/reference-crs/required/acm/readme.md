# Installation instructions

1. Create the `acmNS.yaml` `acmOperGroup.yaml` `acmSubscription.yaml`.
2. If Subscription was set to Manual installPlanApproval, approve the created InstallPlan on `open-cluster-management`
3. Create the `acmMCH.yaml`.
4. If Subscription was set to Manual installPlanApproval, approve the created InstallPlan on `multicluster-engine`
5. Apply the `acmProvisioning.yaml`.
6. Create the `acmAgentServiceConfig.yaml` (Two PVs are required, so ODF must be configured prior to this step).
7. The `multicluster-engine` enables the `cluster-proxy-addon` feature by default. Apply the following patch to disable it: `oc patch multiclusterengines.multicluster.openshift.io multiclusterengine --type=merge --patch-file ./disable-cluster-proxy-addon.json`.
8. Create the `observabilityNS.yaml`.
9. Create the pull-secret. There are two methods to create the pull-secret:
    - The pull-secret multiclusterhub-operator-pull-secret can be automatically created by the ACM policy in pull-secret-copy.yaml. If secret multiclusterhub-operator-pull-secret exists in open-cluster-management, the policy copy it to ns open-cluster-management-observability. If the previous command returns an empty value, then copy secret pull-secret from ns openshift-config.
    - If you want to use your own pull-secret, you may update the value of .dockerconfigjson in observabilitySecret.yaml.
10. Create the `observabilityOBC.yaml`.
11. The Thanos secret will be automatically created by the ACM Policy
    in `thanosSecret.yaml`.
    - The `bucket` and the `endpoint` are copied from the ConfigMap
      that the OBC automatically creates in its namespace. The policy
      pulls the bucket name and host from the fields `BUCKET_NAME`
      (without any protocol or port specification) and `BUCKET_HOST`
      respectively.
    - The `access_key` and the `secret_key` are copied from the Secret
      that the OBC creates automatically in its namespace. The fields
      `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` are pulled from
      the secret and base64 decoded before being inserted into the
      Thanos secret.
12. Create the `observabilityMCO.yaml`.
13. When all the installation is done. Apply the `acmPerfSearch.yaml` .This will configure Search CR called `search-v2-operator` considering different performance and scale optimizations.
